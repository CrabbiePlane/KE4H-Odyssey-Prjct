# KE4H Project: The Odyssey Emotion Graph
**Project by Zurab Kerimov**

## Menu / Navigation
* [1. Topic and Gap Identification](#1-topic-and-gap-identification)
* [2. Methodology and Tools](#2-methodology-and-tools)
* [3. SPARQL Queries](#3-sparql-queries)
* [4. LLM Prompting & Results](#4-llm-prompting--results)
* [5. Challenges & LLM Comparison](#5-challenges--llm-comparison)

---

## 1. Topic and Gap Identification
This project focuses on the combined use of knowledge graphs and Large Language Models (LLMs) applied to Homer's *Odyssey*. While traditional Knowledge Graphs (like DBpedia) successfully map genealogical ties and geographical locations, they lack the **emotional and psychological context** of narrative events. The "gap" identified here is the absence of formalized emotional states during critical encounters. I aimed to extract these emotions from unstructured text and represent them as RDF triples to enrich the graph.

## 2. Methodology and Tools
The general methodology involves:
1. **Knowledge Extraction:** Using LLMs (ChatGPT and Google Gemini) to extract emotional states from text snippets of the Odyssey.
2. **Ontology Modeling:** Extending the standard vocabulary using RDF Schema (RDFS). I defined new classes and subset relationships, and established rules for our custom properties. Below is a snippet of our RDFS logic written in Turtle:

```turtle
# 1. Class Taxonomy
ex:MythologicalFigure a rdfs:Class .
ex:Hero rdfs:subClassOf ex:MythologicalFigure .
ex:Monster rdfs:subClassOf ex:MythologicalFigure .

# 2. Property Constraints (Domain & Range)
ex:facesTrial a rdf:Property .
rdfs:domain ex:Hero .
rdfs:range ex:Monster .
```

3. **Querying:** Using SPARQL 1.1 to explore the graph and generate new knowledge.

*Tools used:* GitHub Pages for publishing, Turtle for RDF serialization, and standard vocabularies (DBpedia/dbo).

## 3. SPARQL Queries
### Query 1: Data Exploration with Mandatory Keywords
This query retrieves distinct characters, finding both Heroes and Monsters, optionally finding their enemies, and filtering the results. It includes all mandatory SPARQL keywords: OPTIONAL, DISTINCT, UNION, FILTER, REGEX, LIMIT, ORDER BY.

```sparql
PREFIX dbr: [http://dbpedia.org/resource/](http://dbpedia.org/resource/)
PREFIX dbo: [http://dbpedia.org/ontology/](http://dbpedia.org/ontology/)
PREFIX ex: [http://example.org/odyssey/](http://example.org/odyssey/)

SELECT DISTINCT ?character ?enemy
WHERE {
    { ?character a ex:Hero . }
    UNION
    { ?character a ex:Monster . }
    
    OPTIONAL { ?character dbo:enemy ?enemy . }
    
    FILTER(REGEX(str(?character), "o", "i"))
}
ORDER BY ?character
LIMIT 10
```

### Query 2: Generating New Knowledge (CONSTRUCT)
This query infers new triples based on existing RDFS logic.

```sparql
PREFIX dbr: [http://dbpedia.org/resource/](http://dbpedia.org/resource/)
PREFIX ex: [http://example.org/odyssey/](http://example.org/odyssey/)
CONSTRUCT {
    ?hero ex:provedHeroism "true" .
}
WHERE {
    ?hero ex:facesTrial ?monster .
}
```

## 4. LLM Prompting & Results
I tested three prompting techniques across two LLMs to extract triples from the following text:
"Odysseus and his men are trapped in a cave. Polyphemus, a cruel Cyclops, devours two sailors. Odysseus is horrified but keeps his cool, devises a plan with a stake."

### Technique 1: Zero-Shot
**Prompt**: Extract the emotions of the characters from the text above. Present the result strictly in RDF Turtle format using the prefix ex:. Subject = character, predicate = ex:feelsEmotion, object = emotion.

**ChatGPT**: Generated invalid Turtle (missing prefixes, used non-standard ex:Odysseus).

**Gemini**: Generated invalid Turtle (missing prefixes, capitalized emotions without proper syntax).

### Technique 2: Few-Shot
**Prompt**: (Included specific RDF examples showing the use of dbr: and ex:feelsEmotion).

**ChatGPT**: Corrected subjects to standard dbr:, but missed @prefix document declarations.

**Gemini**: Successfully output complete, valid Turtle code with all necessary @prefix declarations.

### Technique 3: Chain-of-Thought
**Prompt**: Analyze the text step-by-step. 1) Identify characters. 2) Analyze state. 3) Map emotion. 4) Generate RDF Turtle.

**ChatGPT**: Substituted "Horror" for the more fundamental "Fear" based on reasoning.

**Gemini**: Conducted a deep semantic analysis, identifying that "Cruelty" for Polyphemus is a disposition rather than a temporary emotional state, and successfully avoided hallucinations for the unnamed sailors.

## 5. Challenges & LLM Comparison
**Challenges Faced**: The main challenge was ensuring LLMs output syntactically valid RDF (Turtle) code. Models frequently hallucinated namespaces or forgot prefix declarations, which makes the resulting graph unreadable by machines.

**Difference in Behavior**: ChatGPT proved adequate at basic text extraction but struggled with semantic web standards without explicit guidance. Gemini demonstrated superior performance in the Few-Shot setting by independently inferring the need for document headers (@prefix). In the Chain-of-Thought setting, Gemini showed advanced semantic reasoning by accurately distinguishing between momentary emotions and inherent character traits (dispositions).
