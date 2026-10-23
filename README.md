# Awesome LLM Evals [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

Open-source tools for finding out whether a language model, prompt, RAG pipeline or agent actually works: benchmark
harnesses, test frameworks for your own app, LLM-as-judge, red teaming, and the benchmarks worth running.

Every entry is open source, actively maintained and checked by hand. Each section starts with where to begin.

## Contents

- [Which tool do I need?](#which-tool-do-i-need)
- [Benchmark harnesses](#benchmark-harnesses)
- [Quantized models](#quantized-models)
- [Testing your own application](#testing-your-own-application)
- [RAG evaluation](#rag-evaluation)
- [Tracing and online evaluation](#tracing-and-online-evaluation)
- [LLM as a judge](#llm-as-a-judge)
- [Red teaming and safety](#red-teaming-and-safety)
- [Benchmarks](#benchmarks)
  - [General knowledge and reasoning](#general-knowledge-and-reasoning)
  - [Coding and software engineering](#coding-and-software-engineering)
  - [Agents and tool use](#agents-and-tool-use)
  - [Embeddings and retrieval](#embeddings-and-retrieval)
  - [Hallucination](#hallucination)
- [Habits that keep evals honest](#habits-that-keep-evals-honest)
- [Contributing](#contributing)

## Which tool do I need?

| You want to | Start with |
|---|---|
| Score a model on standard benchmarks, comparably with published numbers | [lm-evaluation-harness](#benchmark-harnesses) |
| Write your own evals for a model or agent, including multi-turn and tool use | [Inspect](#benchmark-harnesses) |
| Test prompts and app outputs in CI, like unit tests | [promptfoo](#testing-your-own-application) or [DeepEval](#testing-your-own-application) |
| Measure a RAG pipeline (retrieval quality, faithfulness) | [Ragas](#rag-evaluation) |
| Watch quality in production and evaluate real traces | [Langfuse](#tracing-and-online-evaluation), [Phoenix](#tracing-and-online-evaluation) or [Opik](#tracing-and-online-evaluation) |
| Probe a model or app for jailbreaks and prompt injection | [garak](#red-teaming-and-safety) or [PyRIT](#red-teaming-and-safety) |
| Pick which quant of a local model to run | [quantpicker](#quantized-models) |

## Benchmark harnesses

Run many standard benchmarks the same way across models.

- [lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) - Hundreds of tasks across Hugging Face, vLLM, SGLang and API backends; the reference implementation behind many leaderboards. Good default.
- [Inspect](https://github.com/UKGovernmentBEIS/inspect_ai) - Framework from the UK AI Security Institute for writing evals in Python, with strong support for agents, tools and sandboxes. Good default for custom evals.
- [HELM](https://github.com/stanford-crfm/helm) - Stanford's holistic evaluation across many scenarios and metrics beyond accuracy, with a public results browser.
- [OpenCompass](https://github.com/open-compass/opencompass) - Large evaluation platform with many datasets and distributed evaluation, strong on Chinese and multilingual benchmarks.
- [Lighteval](https://github.com/huggingface/lighteval) - Hugging Face's toolkit for fast evaluation across backends, used for their leaderboards and model releases.
- [lmms-eval](https://github.com/EvolvingLMMs-Lab/lmms-eval) - The equivalent of lm-evaluation-harness for multimodal models: images, video and audio.
- [Evalchemy](https://github.com/mlfoundations/evalchemy) - Runs many reasoning and chat benchmarks behind one interface, built on top of existing harnesses.
- [simple-evals](https://github.com/openai/simple-evals) - OpenAI's small, readable reference implementations of common benchmarks with zero-shot chat prompts.
- [OpenAI Evals](https://github.com/openai/evals) - Framework and registry of evals from OpenAI, with templates for model-graded and exact-match evals.

## Quantized models

How much a quant (Q4_K_M, IQ4_XS, ...) loses against the full model, measured rather than guessed.

- [llama.cpp perplexity](https://github.com/ggml-org/llama.cpp/tree/master/tools/perplexity) - llama.cpp's own tool for perplexity and KL divergence against a reference model's logits. The standard way to compare GGUF quants.
- [quantpicker](https://github.com/alleyxd/quantpicker) - Runs that comparison plus llama-bench for every quant in a Hugging Face repo on your GPU and recommends the fastest one within a quality limit. `pip install quantpicker`.

## Testing your own application

Treat prompts and LLM features like code: test cases, assertions, CI.

- [promptfoo](https://github.com/promptfoo/promptfoo) - Declarative test cases for prompts, models, RAG and agents, run from the CLI or CI, with red teaming built in. Good default.
- [DeepEval](https://github.com/confident-ai/deepeval) - Pytest-style unit tests for LLM apps with ready-made metrics for relevance, faithfulness, hallucination and more.
- [Giskard](https://github.com/Giskard-AI/giskard-oss) - Automatic scans of LLM agents for hallucination, prompt injection and harmful output, plus test suites.
- [AutoEvals](https://github.com/braintrustdata/autoevals) - Small library of scoring functions (LLM-graded, heuristic and statistical) you can drop into any test.
- [Hugging Face Evaluate](https://github.com/huggingface/evaluate) - Standard metric implementations (BLEU, ROUGE, exact match and many more) with one API.
- [Scorio](https://github.com/mohsenhariri/scorio) - Scores recorded outcomes from repeated LLM trials, with Bayesian performance estimates, uncertainty, and weighted rubrics.

## RAG evaluation

- [Ragas](https://github.com/vibrantlabsai/ragas) - Metrics for retrieval and generation such as context precision, recall and faithfulness, plus synthetic test set generation. Good default.
- [TruLens](https://github.com/truera/trulens) - Feedback functions (groundedness, context relevance, answer relevance) tracked across app versions.

## Tracing and online evaluation

See what your application actually did, then score real traffic.

- [Langfuse](https://github.com/langfuse/langfuse) - Tracing, prompt management, datasets and evaluations, self-hostable. Good default.
- [Phoenix](https://github.com/Arize-ai/phoenix) - OpenTelemetry-based tracing and evaluation that runs locally in a notebook or as a server.
- [Opik](https://github.com/comet-ml/opik) - Tracing, automated evaluation and production monitoring for LLM apps, RAG and agents.

## LLM as a judge

Using a model to grade outputs. Validate the judge against human labels on a sample before trusting it.

- [Prometheus-Eval](https://github.com/prometheus-eval/prometheus-eval) - Open judge models and code for rubric-based absolute and pairwise grading.
- [AlpacaEval](https://github.com/tatsu-lab/alpaca_eval) - Automatic pairwise evaluator for instruction following, with length-controlled win rates.
- [Arena-Hard-Auto](https://github.com/lmarena/arena-hard-auto) - Hard prompts from Chatbot Arena graded by a strong judge, built to track human preference rankings.
- [FastChat](https://github.com/lm-sys/FastChat) - Home of MT-Bench, the multi-turn benchmark graded by an LLM judge.

## Red teaming and safety

- [garak](https://github.com/NVIDIA/garak) - Vulnerability scanner that probes models for jailbreaks, prompt injection, data leakage and toxic output. Good default.
- [PyRIT](https://github.com/microsoft/PyRIT) - Microsoft's framework for automated red teaming of generative AI systems, including multi-turn attacks.

## Benchmarks

### General knowledge and reasoning

- [MMLU](https://github.com/hendrycks/test) - The classic multiple-choice test across 57 subjects. Saturated for frontier models, still useful for small ones.
- [GPQA](https://github.com/idavidrein/gpqa) - Graduate-level science questions written to be hard to answer by searching.
- [LiveBench](https://github.com/LiveBench/LiveBench) - Questions refreshed regularly from recent sources to limit training-data contamination.

### Coding and software engineering

- [SWE-bench](https://github.com/SWE-bench/SWE-bench) - Resolve real GitHub issues in real repositories, checked by the projects' own tests.
- [SWE-agent](https://github.com/SWE-agent/SWE-agent) - Reference agent and harness for running models on SWE-bench style tasks.
- [LiveCodeBench](https://github.com/LiveCodeBench/LiveCodeBench) - Coding problems collected after model release dates to limit contamination.
- [HumanEval](https://github.com/openai/human-eval) - Function-completion problems checked by unit tests; the original code benchmark.
- [BigCode Evaluation Harness](https://github.com/bigcode-project/bigcode-evaluation-harness) - Runs many code generation benchmarks with sandboxed execution.
- [EvalPlus](https://github.com/evalplus/evalplus) - HumanEval and MBPP with many times more tests per problem, which catches code that only passes the original few.

### Agents and tool use

- [tau2-bench](https://github.com/sierra-research/tau2-bench) - Agents talking to simulated users while following domain policies and calling tools.
- [Terminal-Bench](https://github.com/harbor-framework/terminal-bench) - Hard real-world tasks an agent has to finish in a terminal, from compiling code to training models, each checked by tests.
- [Berkeley Function Calling Leaderboard](https://github.com/ShishirPatil/gorilla) - Function and tool calling accuracy across many models (in the Gorilla repository).
- [AgentBench](https://github.com/THUDM/AgentBench) - LLMs as agents across operating system, database, web and game environments.
- [OpenAI Frontier Evals](https://github.com/openai/frontier-evals) - OpenAI's open evals for frontier capabilities, such as reproducing research.

### Embeddings and retrieval

- [MTEB](https://github.com/embeddings-benchmark/mteb) - The standard benchmark for text embeddings across retrieval, clustering, classification and many languages.

### Hallucination

- [Hallucination Leaderboard](https://github.com/vectara/hallucination-leaderboard) - How often models add unsupported facts when summarizing a given document.

## Habits that keep evals honest

Lessons that apply whatever tool you use:

- **Look at outputs, not only scores.** Read a sample of failures for every eval; most broken evals are found this way.
- **Check the prompt the model actually saw.** Template bugs (a missing field, a literal `\n`) silently change what is measured.
- **Report the sample size.** On 300 examples, a 3-point difference is within noise.
- **Keep test data out of training data.** Check prompt overlap before you trust a fine-tuning gain.
- **Validate your judge.** Compare an LLM judge with human labels on a sample before relying on it.
- **Measure calibration, not just accuracy,** if anyone will act on the model's confidence.

## Contributing

Suggestions are welcome. A project fits if it is open source, maintained (commits in the last year, or a widely used
benchmark), and helps evaluate language models or LLM applications. Open a pull request with one line in the matching
section: name, link, and what it does in plain words. Links are checked every week.

## License

[CC0 1.0](LICENSE). To the extent possible under law, the authors have waived all copyright and related rights.
