# HCIS L7 Revision Quiz

A self-contained, multiple-choice revision quiz. It runs entirely in the browser, loads its questions from a single JSON file, and costs nothing to host. There is no server, no database, and no learner data is collected or stored: every attempt is fresh and private to the learner's own browser.

## What is in this repo

- `index.html` is the quiz itself (the engine and styling). You rarely need to touch this.
- `questions.json` is the question bank. This is the file you edit to add or change questions.
- `README.md` is this file.

## How learners use it

Once deployed, give learners the page URL. They start the quiz, answer one question at a time, and get feedback with a short rationale after each answer (the answer is not shown until they commit). At the end they see a score and a breakdown by KSB showing which areas to revise.

## Deploying it (GitHub Pages, free)

1. Put `index.html` and `questions.json` into a **public** GitHub repository.
2. In `index.html`, set the config line near the top to `const QUESTION_BANK_URL = "questions.json";` so the page loads the bank that sits beside it.
3. In the repo, go to **Settings**, then **Pages**, set the source to the `main` branch and the root folder, and save.
4. After a minute, the Pages panel shows your live URL, of the form `https://YOURNAME.github.io/REPO/`. That single link is what you give learners.

To update questions later, edit `questions.json` in the repo and commit. The live quiz picks up the change within a minute or two. You never need to touch `index.html` again.

## Editing the question bank

`questions.json` is a list of question objects. One looks like this:

```json
{
  "id": "q01",
  "ksb": ["K4"],
  "stem": "The question text goes here.",
  "options": ["First option", "Second option", "Third option", "Fourth option"],
  "answer": 0,
  "rationale": "The explanation shown to the learner after they answer."
}
```

What each field means:

- `id`: any short unique label for the question.
- `ksb`: a list of KSB codes, always in square brackets even for a single code (`["K4"]`). A question can carry more than one (`["K4", "S2"]`), and it then counts towards each of them in the results breakdown.
- `options`: the answer choices. Four is typical, but any number works.
- `answer`: the position of the correct option, counting from zero, so `0` is the first option and `3` is the fourth. This is the single easiest thing to get wrong, so check each one.
- `rationale`: the feedback shown after answering. This is what turns the quiz into a revision aid rather than a test, so make it worth reading.

Three rules keep the file valid:

- Use straight double quotes (`"`), not curly ones. Do not draft questions in Word, which converts them silently and breaks the file. Use a plain text editor such as VS Code.
- Put commas between items, but never after the last item in a list or object.
- Validate before committing. If the file fails to parse, the quiz quietly falls back to its built-in sample and your changes will appear to do nothing. VS Code underlines JSON errors as you type, or you can paste the file into any free online JSON validator.

## Configuration

All settings sit in a clearly marked block near the top of `index.html`.

- `QUESTION_BANK_URL`: set to `"questions.json"` to load the bank from the repo, or `null` to use the built-in sample.
- `QUIZ_LENGTH`: how many questions to draw per attempt when stratified sampling is off.
- `WEAK_THRESHOLD`: a KSB scoring below this fraction (`0.6` means 60%) is flagged for review on the results screen.
- `STRATIFY_BY_KSB`: off by default. When `true`, the quiz ignores `QUIZ_LENGTH` and instead draws `PER_KSB` questions from each code in `KSB_LIST`, giving an even spread across the standard. Turn this on once the bank holds enough questions per KSB to support it.
- `KSB_LIST`: the KSB codes to draw from when stratified sampling is on.
- `PER_KSB`: how many questions to draw from each KSB when stratified sampling is on.

To rebrand the look, change the `--accent` colour variable in the style block near the top of `index.html`.

## Testing locally

Opening `index.html` straight from your hard drive will not load `questions.json`, because browsers block local file reads for security, so it falls back to the sample bank. To test the real bank on your own machine, run `python -m http.server` in the folder and open the address it prints. On the live Pages URL it always works.

## A note on KSB codes

The codes in the demo bank (`K1`, `K5`, `K8`, `S3`, `B2`) are placeholders. Map them to the real HCIS L7 standard before going live.