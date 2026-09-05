# Grill Skills Repository

This repository contains three specialized skills for code understanding and review: `grill-code`, `grill-diff`, and `grill-phone`. These skills are designed to rigorously test a user's understanding of code through targeted questioning, ensuring deep comprehension rather than surface-level knowledge.

## Skills Overview

### grill-code
The `grill-code` skill is designed to relentlessly quiz users on specific pieces of code — a file, directory, or symbol — to test whether they actually understand it. This includes testing behavior, rationale, contracts, edge cases, blast radius, correctness, and security.

**Key Features:**
- Tests understanding through comprehensive questioning
- Focuses on deep comprehension rather than surface knowledge
- Uses a structured "comprehension tree" approach with multiple dimensions
- Provides detailed feedback and scorecards after assessment
- Works with files, directories, or specific symbols

### grill-diff
The `grill-diff` skill quizzes users on code changes in git diffs to test their understanding of what was changed, why it was changed, and the implications of those changes.

**Key Features:**
- Tests understanding of code modifications in git diffs
- Analyzes both the problem being fixed and the solution implemented
- Considers behavior, rationale, contract, failure cases, blast radius, tradeoffs, and correctness/security
- Works with various diff specifications (main, HEAD~3, --staged, ranges, paths)
- Provides detailed assessment of change impact

### grill-phone
The `grill-phone` skill is designed to relentlessly quiz users on specific pieces of code — a file, directory, or symbol — by sending code snippets to the user's terminal/device for examination. This allows testing understanding while providing hands-on code analysis.

**Key Features:**
- Tests understanding through comprehensive questioning
- Focuses on deep comprehension rather than surface knowledge
- Uses a structured "comprehension tree" approach with multiple dimensions
- Sends code snippets to terminal/device for user examination
- Provides detailed feedback and scorecards after assessment
- Works with files, directories, or specific symbols

## Installation

### Installing in Claude

1. **Access the Skills Marketplace**
   - Open Claude on [claude.ai](https://claude.ai)
   - Navigate to the "Skills" section in your settings
   - Click on "Browse Skills"

2. **Find and Install the Skills**
   - Search for "grill-code", "grill-diff", or "grill-phone"
   - Click "Install" on each skill you want to use

3. **Using the Skills**
   - In a conversation, mention "grill me on this code" or "quiz me on <thing>"
   - Or invoke `/grill-code` or `/grill-diff` directly
   - Provide specific targets for the skills (file paths, directories, symbols, or diff specs)

### Installing with GitHub CLI

You can also install these skills using the GitHub CLI:

- Install with: `gh skill install kostya253/grill-me-diff`
- Pin with: `gh skill install kostya253/grill-me-diff <skill> --pin v1.0.0`

### Installing in Codex

1. **Access the Skills Marketplace**
   - Open your Codex environment
   - Navigate to the "Skills" section
   - Click on "Browse Skills"

2. **Find and Install the Skills**
   - Search for "grill-code", "grill-diff", or "grill-phone"
   - Click "Install" on each skill you want to use

3. **Using the Skills**
   - In a conversation, mention "grill me on this code" or "quiz me on <thing>"
   - Or invoke `/grill-code` or `/grill-diff` directly
   - Provide specific targets for the skills (file paths, directories, symbols, or diff specs)

### Installing in OpenCode

1. **Access the Skills Marketplace**
   - Open your OpenCode environment
   - Navigate to the "Skills" section
   - Click on "Browse Skills"

2. **Find and Install the Skills**
   - Search for "grill-code", "grill-diff", or "grill-phone"
   - Click "Install" on each skill you want to use

3. **Using the Skills**
   - In a conversation, mention "grill me on this code" or "quiz me on <thing>"
   - Or invoke `/grill-code` or `/grill-diff` directly
   - Provide specific targets for the skills (file paths, directories, symbols, or diff specs)

## Usage Examples

### Using grill-code
```
User: "grill me on this code/file"
Assistant: "What specific file, directory, or symbol would you like me to grill you on?"
User: "grill me on src/main.py"
Assistant: "I'll grill you on the main.py file. Let's begin..."
```

### Using grill-diff
```
User: "grill me on the diff"
Assistant: "What specific diff would you like me to grill you on? (e.g., main, HEAD~3, --staged)"
User: "grill me on my changes"
Assistant: "I'll grill you on your recent changes. Let's begin..."
```

### Using grill-phone
```
User: "grill me on this code/file"
Assistant: "What specific file, directory, or symbol would you like me to grill you on?"
User: "grill me on src/main.py"
Assistant: "I'll grill you on the main.py file. Let's begin... (sending code snippets to terminal for examination)"
```

## Terminal Execution Features

The `grill-phone` skill includes a unique feature where it sends code-related questions to the terminal for actual execution:

- Questions that require code behavior testing are automatically sent to the terminal
- Results from terminal execution are analyzed and used to grade user answers
- This provides real-world validation of code understanding

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.
