Markdown
# Knowledge Engineering for the Humanities: The Odyssey Emotion Graph

**Team Members:** [Zurab Kerimov]

## Navigation
* [Methodology & Gap Identification](#methodology)
* [LLM Prompting & Results](#llm)
* [SPARQL Queries](#sparql)
* [Challenges & Discussion](#challenges)

---

<h2 id="methodology">1. Topic & Gap Identification</h2>
Our project focuses on Homer's Odyssey. While standard knowledge graphs (like DBpedia) successfully map the locations and family ties of the characters[cite: 3], they fail to capture the **emotional context** of key narrative events. Our methodology aims to fill this gap by using Large Language Models to extract emotions from text and formalize them into RDF triples[cite: 1].

<h2 id="llm">2. Prompting Techniques & LLM Comparison</h2>
We utilized three prompting techniques (Zero-shot, Few-shot, Chain-of-Thought) across two models (ChatGPT and Gemini). 
*(Здесь вставь свои выводы о том, какая модель лучше справилась с форматированием Turtle и кто нашел более глубокие эмоции).*

<h2 id="sparql">3. SPARQL Queries</h2>
We designed several queries to retrieve data from our graph. Below is an example utilizing multiple required operators (OPTIONAL, FILTER, etc.):
*(Здесь вставь блок кода со своим SPARQL-запросом).*

<h2 id="challenges">4. Challenges and Discussion</h2>
*(Здесь опиши трудности: например, как LLM ошибались в синтаксисе RDF или как сложно было подобрать правильный словарь для описания эмоций).*
