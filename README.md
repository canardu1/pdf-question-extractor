# PDF Question Extractor

A Python tool that extracts text from PDF documents and generates multiple-choice questions in Aiken format using LLaMA 3.2, with options for validation and GIFT format conversion.

## Features

- PDF text extraction with chunk processing
- Question generation using LLaMA 3.2 model via Ollama
- Question validation and improvement
- Conversion to GIFT format with detailed feedback
- Aiken and GIFT format output
- Colored progress tracking
- Detailed logging

## Requirements

- Python 3.8+
- PyPDF2
- requests
- tqdm
- Ollama with LLaMA 3.2 model installed

## Installation

1. Clone the repository:
```bash
git clone [repository-url]
cd pdf-question-extractor
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Install Ollama and download the LLaMA 3.2 model:
```bash
# Install Ollama (Mac/Linux)
curl https://ollama.ai/install.sh | sh

# Pull the LLaMA 3.2 model
ollama pull llama3.2
```

## Usage

### 1. Generate Questions

Generate multiple-choice questions from a PDF file:

```bash
python main.py path/to/your.pdf --num-questions 50 --output questions.txt
```

Options:
- `--num-questions`: Number of questions to generate (default: 10)
- `--chunk-size`: Size of text chunks for processing (default: 8000)
- `--output`: Output file for questions (default: questions.txt)
- `--debug`: Enable debug logging

### 2. Validate and Improve Questions

Run a second pass to validate and improve the generated questions:

```bash
python second_passage.py path/to/your.pdf questions.txt --output questions_improved.txt
```

Options:
- `--chunk-size`: Size of text chunks for processing (default: 8000)
- `--output`: Output file for improved questions (default: questions_improved.txt)
- `--debug`: Enable debug logging

The validation process:
1. Loads the original questions
2. For each question:
   - Finds the most relevant context from the PDF
   - Validates the question against this context
   - Either approves it or suggests improvements
3. Saves improved questions to a new file (original file remains unchanged)

### 3. Convert to GIFT Format

Convert the improved questions to GIFT format with detailed feedback:

```bash
python gift_converter.py path/to/your.pdf questions_improved.txt --output questions.gift --batch-size 500 --show-gift
```

Options:
- `--chunk-size`: Size of text chunks for processing (default: 8000)
- `--output`: Base name for output files (default: questions.gift)
- `--batch-size`: Number of questions per output file (default: 500)
- `--show-gift`: Display converted questions in output
- `--debug`: Enable debug logging

The conversion process:
1. Loads the improved Aiken format questions
2. For each question:
   - Finds relevant context from the PDF
   - Generates detailed feedback for correct and incorrect answers
   - Includes direct quotations and law references
   - Shows the original Aiken and converted GIFT formats (if --show-gift is used)
3. Saves questions in GIFT format:
   - Creates separate files for every 500 questions
   - Names files as questions_1.gift, questions_2.gift, etc.
   - Original file remains unchanged

## Output Formats

### Aiken Format (Steps 1 & 2)
```
What is the capital of France?
A. London
B. Berlin
C. Paris
D. Madrid
ANSWER: C
```

### GIFT Format (Step 3)
```
::Q:: What is the capital of France? {
=Paris # Correct! Paris is indeed the capital of France, as established in...
~London # Incorrect. London is the capital of the United Kingdom, not France.
~Berlin # Incorrect. Berlin is the capital of Germany, not France.
~Madrid # Incorrect. Madrid is the capital of Spain, not France.
}
```

## Project Structure

- `main.py`: Main script for generating questions
- `second_passage.py`: Script for validating and improving questions
- `gift_converter.py`: Script for converting to GIFT format with feedback
- `pdf_extractor.py`: PDF text extraction utilities
- `question_generator.py`: Question generation using LLaMA
- `utils.py`: Utility functions for text processing
- `requirements.txt`: Project dependencies

## Error Handling

The tool includes comprehensive error handling for:
- PDF reading errors
- API communication issues
- Invalid question formats
- Missing context
- Format conversion issues

Errors are logged with colored output for better visibility.

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

[Your License]
