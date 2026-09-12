# Knowledge Engineering for the Humanities: The Odyssey Emotion Graph

**Team Members:** [Zurab Kerimov]

## Navigation
* [Methodology & Gap Identification](#methodology)
* [LLM Prompting & Results](#llm)
* [SPARQL Queries](#sparql)
* [Challenges & Discussion](#challenges)

---

<h2 id="methodology">1. Topic & Gap Identification</h2>
My project focuses on Homer's Odyssey. While standard knowledge graphs (like DBpedia) successfully map the locations and family ties of the characters, they fail to capture the 'emotional context' of key narrative events. My methodology aims to fill this gap by using Large Language Models to extract emotions from text and formalize them into RDF triples.

<h2 id="llm">2. Prompting Techniques & LLM Comparison</h2>
I utilized three prompting techniques (Zero-shot, Few-shot, Chain-of-Thought) across two models (ChatGPT and Gemini). 

Syntax and Invalid Code (Zero-Shot): Without examples, both models produced invalid code. They forgot to declare namespaces, which act as prefixes for shortening URIs. Without the @prefix declaration, the machine would be unable to read these triplets. Furthermore, the models generated ex:Odysseus instead of using standard URIs from DBpedia, which violates the fundamental principle of uniquely identifying Semantic Web objects.

Impact of Examples on Standardization (Few-Shot): As soon as the models were given a template, they immediately corrected the subjects to the correct unique identifiers dbr:Odysseus. This is where the difference between the models became apparent: Gemini understood the context more deeply and automatically added the necessary @prefix declarations, producing fully usable code. ChatGPT limited itself to the triplets themselves, ignoring the document structure.

Semantic Precision (Chain-of-Thought): Step-by-step reasoning dramatically improved the quality of knowledge extracted. ChatGPT conceptualized "horror," replacing it with the more fundamental emotion "Fear." Gemini demonstrated superior analytics: it refused to invent emotions for the eaten sailors (avoiding hallucinations) and raised an important ontological question: whether Polyphemus's "Cruelty" is an emotion or a persistent state/characteristic (disposition).

Basic prompts are not enough to build a high-quality graph. The Few-Shot technique is critical to maintaining the strict format of the RDF data model, and Chain-of-Thought is necessary for semantic purity, so that the model does not confuse temporary emotions with the permanent characteristics of characters.

<h2 id="sparql">3. SPARQL Queries</h2>
I designed several queries to retrieve data from our graph. Below is an example utilizing multiple required operators (OPTIONAL, FILTER, etc.):
*(Здесь вставь блок кода со своим SPARQL-запросом).*

<h2 id="challenges">4. Challenges and Discussion</h2>
*(Здесь опиши трудности: например, как LLM ошибались в синтаксисе RDF или как сложно было подобрать правильный словарь для описания эмоций).*
