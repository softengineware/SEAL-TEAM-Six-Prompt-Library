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

## Directories

### mixed_prompts/

This directory contains the output of the prompt mixer tool. All mixed prompts are stored here by default.

## Future Enhancements

Planned features for these scripts:
- Statistics generation for prompt files (token count, complexity score, etc.)
- Integration with LLM APIs to test prompts automatically
- Generating visualization of prompt relationships and common patterns
- Building a web interface for interactive prompt creation and editing 