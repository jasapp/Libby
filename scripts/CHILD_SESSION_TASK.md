# Child session task: look up library websites for one chunk

You are one of several sessions filling in official websites for US public libraries. Your chunk number is given in your prompt as N.

1. Read scripts/LOOKUP_INSTRUCTIONS.md (the per-library procedure your helper agents must follow).
2. Your input is data/chunks/chunk_N.json. Split it into pieces of 50 libraries each: data/chunks/chunk_N_p00.json, chunk_N_p01.json, ... (write these with a short Python script).
3. Run helper agents (the Agent tool, model sonnet) on the pieces, 5 at a time in parallel. Each helper's prompt must say: read scripts/LOOKUP_INSTRUCTIONS.md and follow it exactly; input file data/chunks/chunk_N_pXX.json; output file data/results/chunk_N_pXX.jsonl. Wait for each wave to finish before launching the next.
4. After every wave: `git add data/results && git commit -m "Library websites: chunk N partial results" && git push -u origin <your outcome branch>` so progress is saved even if the session dies. Use `git push` retries with backoff on network errors.
5. If helpers report the web search budget ran out, do not launch more helpers. Commit and push whatever results exist and finish.
6. Finish with a report: chunk number, libraries searched, websites found, blanks, and whether the search budget ran out (and after roughly how many searches).

Do not edit any other files. Do not create a pull request. Never write placeholder result lines for libraries that were not actually searched.
