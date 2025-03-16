# LLM Prompt Library Scripts

This directory contains utility scripts to help maintain and enhance the LLM Prompt Library.

## Available Scripts

### 1. Prompt Validator (`validate_prompts.py`)

This script validates the format and contents of prompt files to ensure they meet the repository's standards.

#### Usage

```bash
# Basic usage (checks all prompts in the prompts/ directory)
python scripts/validate_prompts.py

# Validate prompts in a specific directory
python scripts/validate_prompts.py --dir prompts/programming

# Get detailed output about each file
python scripts/validate_prompts.py -v

# Use strict validation mode (more rigorous checks)
python scripts/validate_prompts.py -s
```

The validator checks each prompt file for:
- Proper title format (starts with # Title)
- Presence of a markdown code block
- Sufficient configuration options (`reset`, `no quotes`, etc.)
- Clear instructions within the prompt
- Adequate length and content

By default, the validator operates in a lenient mode that focuses on critical issues while providing warnings for minor issues. The strict mode (`-s` flag) applies more rigorous criteria.

If issues are found, the script will list them and exit with a non-zero status code, making it suitable for CI/CD pipelines.

### 2. Prompt Mixer (`prompt_mixer.py`)

This script allows you to create new prompts by mixing and matching elements from existing prompts in the library.

#### Usage

```bash
# Basic usage (creates a random mix using elements from the prompts/ directory)
python scripts/prompt_mixer.py

# Specify a custom title for the mixed prompt
python scripts/prompt_mixer.py --title "My Custom Mixed Prompt"

# Mix specific elements from different prompts
python scripts/prompt_mixer.py \
  --config-from "10-KAnalyzer.md" \
  --instructions-from "programming/Python.md" \
  --examples-from "writing_editing/Proofread.md" \
  --output-from "programming/Code_Explainer.md"

# Specify an output file name
python scripts/prompt_mixer.py --output-file "my_special_mix.md"

# Get detailed output about the mixing process
python scripts/prompt_mixer.py -v
```

The mixer:
1. Scans prompt files and extracts their components (title, configuration options, instructions, examples, output guidance)
2. Allows you to select specific components from different source files
3. Combines these components into a coherent new prompt
4. Saves the result to the `scripts/mixed_prompts/` directory (or a location you specify)
5. Includes source attribution in a comment at the end of the file

The mixer is designed to be robust and can handle a variety of prompt formats, even those not strictly adhering to the recommended structure. It will add default elements where necessary to ensure the mixed prompt is functional.

### 3. Token Counter (`token_counter.py`)

This script analyzes prompt files and counts tokens using various tokenization methods, helping you understand token usage and estimate API costs.

#### Usage

```bash
# Basic usage (analyzes all prompts in the prompts/ directory)
python scripts/token_counter.py

# Analyze prompts in a specific directory
python scripts/token_counter.py --dir prompts/programming

# Analyze a specific file
python scripts/token_counter.py --file prompts/programming/Python.md

# Use a specific tokenizer model
python scripts/token_counter.py --tokenizer gpt-4

# Skip counting tokens in code blocks
python scripts/token_counter.py --skip-code-blocks

# Include markdown formatting in token counts
python scripts/token_counter.py --include-markdown

# Export results to a JSON file
python scripts/token_counter.py --export token_stats.json

# Get detailed output about each file
python scripts/token_counter.py -v
```

The token counter provides:
- Total token count across all prompt files
- Token usage broken down by category
- Identification of the most token-heavy prompts
- Estimated API costs for different LLM models
- Detailed per-file analysis

For accurate tokenization with OpenAI models, the script uses the `tiktoken` library. If not available, it falls back to a simple word-based approximation.

### 4. Prompt Analyzer (`prompt_analyzer.py`)

This script analyzes the quality, readability, and structure of prompts, providing actionable suggestions for improvements.

#### Usage

```bash
# Basic usage (analyzes all prompts in the prompts/ directory)
python scripts/prompt_analyzer.py

# Analyze a specific file
python scripts/prompt_analyzer.py --file prompts/programming/Python.md

# Get detailed output for each file
python scripts/prompt_analyzer.py -v

# Set minimum recommended examples in prompts
python scripts/prompt_analyzer.py --min-examples 2

# Perform more thorough analysis (slower but more detailed)
python scripts/prompt_analyzer.py --thorough

# Export results to a JSON file
python scripts/prompt_analyzer.py --export analysis_results.json
```

The prompt analyzer evaluates each prompt on several dimensions:
- **Readability**: How easy the prompt is to read and understand (sentence length, word complexity, etc.)
- **Structure**: How well-structured the prompt is (title, configuration options, examples, etc.)
- **Clarity**: How clear the instructions are (instruction patterns, complexity, etc.)
- **Overall Quality**: Weighted combination of the above scores

For each analyzed prompt, the script provides:
- Detailed scores across various quality dimensions
- Specific recommendations for improvements
- Metadata about the prompt structure (examples count, code blocks, etc.)
- Keyword analysis (when running in thorough mode with NLTK installed)

The analyzer also provides repository-wide statistics:
- Quality distribution across all prompts
- Comparison of quality across different prompt categories
- Identification of the highest and lowest quality prompts

For optimal results, install the NLTK library (`pip install nltk`), which enables more sophisticated text analysis.

### 5. Prompt Evolution (`prompt_evolution.py`)

This script implements an autonomous prompt optimization system that iteratively refines prompts through self-evolution, critique, and feedback-driven improvement.

#### Usage

```bash
# Basic usage (requires a task description)
python scripts/prompt_evolution.py --task "Summarize scientific papers concisely"

# Start with an initial prompt file
python scripts/prompt_evolution.py --task "Explain complex code" --initial-prompt prompts/programming/Code_Explainer.md

# Run in simulation mode (no API key needed)
python scripts/prompt_evolution.py --task "Write poetry in the style of Emily Dickinson" --simulate

# Customize evolution parameters
python scripts/prompt_evolution.py --task "Generate creative stories" --population 10 --iterations 8

# Use a specific LLM model with your API key
python scripts/prompt_evolution.py --task "Create SQL queries" --model gpt-4 --api-key YOUR_API_KEY

# Get detailed progress information
python scripts/prompt_evolution.py --task "Design marketing copy" --verbose
```

The prompt evolution system employs multiple techniques to create increasingly effective prompts:

- **Evolutionary Algorithms**: Maintains a population of prompts that evolve across generations
- **Self-Critique**: Generates constructive feedback on prompts to guide improvements
- **Mutation Strategies**: Applies various transformations to create diverse prompt variations
- **Quality Evaluation**: Assesses prompt effectiveness using heuristics or LLM feedback

For each evolution process, the script generates:
- The best-performing prompt in markdown format
- A detailed evolution report showing progress across generations
- Complete evolution data in JSON for further analysis

The system works in two modes:
1. **Simulation Mode**: Works without API keys, using heuristic evaluation
2. **LLM-Powered Mode**: Uses OpenAI or Anthropic APIs for more sophisticated evolution (requires API key)

For best results with LLM-powered evolution, install the optional dependencies:
```bash
pip install openai>=1.0.0 anthropic>=0.5.0
```

### 6. Financial Metacognition

The Financial Metacognition module is a specialized tool for analyzing and evaluating AI-generated responses to financial prompts. This tool helps identify potential biases, reasoning limitations, and confidence issues in AI interpretations of financial topics.

### Directory Structure

The Financial Metacognition module is organized in its own dedicated directory:

```
financial_metacognition/
├── financial_metacognition.py - Main analysis script
├── config/ - Configuration files for analysis patterns
│   ├── financial_concepts.json - Financial terminology by region
│   ├── bias_patterns.json - Patterns to detect cognitive biases
│   ├── limitation_patterns.json - Patterns for reasoning limitations
│   └── confidence_patterns.json - Confidence assessment patterns
└── examples/ - Example files for testing
    ├── test_financial_metacognition.py - Test script
    ├── financial_prompt.txt - Example prompt
    └── financial_response.txt - Example response
```

### Features

- **Regional Analysis**: Support for US, EU, and Asian financial terminology and accounting standards
- **Bias Detection**: Identifies potential cognitive biases in financial analysis
- **Reasoning Evaluation**: Assesses limitations in financial reasoning
- **Confidence Assessment**: Evaluates the appropriateness of confidence expressions
- **Recommendation Generation**: Provides actionable suggestions for prompt improvement

### Usage

#### Basic Analysis

```bash
python financial_metacognition/financial_metacognition.py --prompt-file input/prompt.txt --response-file input/response.txt --output analysis.json
```

#### Region-Specific Analysis

```bash
python financial_metacognition/financial_metacognition.py --prompt-file input/prompt.txt --response-file input/response.txt --region EU --output eu_analysis.json
```

#### Running the Test Script

```bash
python financial_metacognition/examples/test_financial_metacognition.py --region US --accounting-standard GAAP
```

### Configuration

The behavior of the financial metacognition module can be customized by modifying the JSON configuration files in the `config/` directory:

- **financial_concepts.json**: Add or modify financial terminology for different regions
- **bias_patterns.json**: Customize patterns for detecting cognitive biases
- **limitation_patterns.json**: Adjust patterns for identifying reasoning limitations
- **confidence_patterns.json**: Modify confidence assessment markers and thresholds

### Dependencies

The Financial Metacognition module requires:
- Python 3.6+
- spaCy with the `en_core_web_lg` model
- Standard libraries: json, re, argparse, os, datetime

Install spaCy and the required model with:
```bash
pip install spacy
python -m spacy download en_core_web_lg
```

## Directories

### mixed_prompts/

This directory contains the output of the prompt mixer tool. All mixed prompts are stored here by default.