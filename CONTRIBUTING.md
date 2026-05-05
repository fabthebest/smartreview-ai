# Contributing to SmartReview AI

Thanks for your interest in contributing.

## How to contribute

### Report issues
If you find a bug or a cell that doesn't run, open an issue with:
- The cell number or section name
- The error message (full traceback)
- Your environment (Colab, local, GPU/CPU)

### Suggest improvements
Open an issue describing what you'd like to change and why. Ideas that are especially welcome:
- New reverse engineering challenges at the end of each phase
- Alternative model comparisons (RoBERTa, ALBERT, etc.)
- Translations of the notebook or README
- Better visualizations

### Submit changes
1. Fork the repository
2. Create a branch (`git checkout -b my-improvement`)
3. Make your changes
4. Test by running the full notebook top to bottom in Colab
5. Submit a pull request with a clear description of what you changed

### Add new phases
Phases 5, 6, and 7 are planned. If you want to contribute a phase:
- Open an issue first to discuss the approach
- Follow the existing structure: working code, comments explaining why, reverse engineering challenges at the end
- Each phase must reuse the same IMDB dataset and build on previous phases

## Code style
- Comments explain WHY, not WHAT
- Every new section starts with a markdown cell explaining the goal
- Code must run in Google Colab with a T4 GPU without modifications

## License
By contributing, you agree that your contributions will be licensed under Apache 2.0.
