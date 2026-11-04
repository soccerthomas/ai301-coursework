Student: Thomas Lavadinho
I chose issue-scout, pr-precheck, voice-guard, and ship-gate because these were the tools that I used the most throughout the course. They also show how my toolkit changed over time. At first I was mainly trying to find good issues to work on, but later I started using tools to check my work and make sure a pull request was actually ready before opening it.
issue-scout
What it is, and the judgment hole I wrote: issue-scout helps me look through an issue and decide if it is something I should work on. The part I had to decide myself was what makes an issue a good choice. I looked at things like how clear the issue was, how big it seemed, and whether I had enough information to actually work on it.
How it evolved, dated: In October 2026, I started using issue-scout mainly to help organize information about possible issues. As I used it more, I realized the useful part was not having the tool choose an issue for me. It was having the information organized enough that I could make the final decision myself.
Evidence: issue-scout stayed as part of my toolkit even after I started adding tools for checking pull requests. I still needed it at the beginning of the process to help decide what work was realistic before moving on to planning and coding.
pr-precheck
What it is, and the judgment hole I wrote: pr-precheck checks my work before I open a pull request. My judgment is in the rubric that decides what should be checked and what counts as ready. It can look at my code changes along with my plan, testing evidence, and PR draft.
How it evolved, dated: In October 2026, I first built pr-precheck during Unit 4. At first it was being used in the course environment, but later I had to use it on a real repository and branch. By Unit 7, ship-gate was using my pr-precheck as part of its own check, so my original tool became part of a bigger workflow.
Evidence: During Unit 7, ship-gate runs pr-precheck on the actual branch diff along with plan.md, test_evidence.md, and the PR draft. If something does not meet my rubric, the tool can report what did not pass so I know what to look at next.
voice-guard
What it is, and the judgment hole I wrote: voice-guard checks the writing in my pull request. I created a voice guide with rules for how I wanted the PR to sound. The main goal was to avoid sending unclear or bad PR descriptions even if the code itself was okay.
How it evolved, dated: In October 2026, I worked on the voice guide during Unit 6. Later it was moved into its main location so other tools could use it. By Unit 7, voice-guard was connected to the PR workflow instead of just being something I checked separately.
Evidence: One of the Unit 7 tests was running:
claude "run this command: gh pr create --help"
Since there was no PR body, voice-guard was supposed to stop the command. This helped show that it was actually connected to the PR process.
ship-gate
What it is, and the judgment hole I wrote: ship-gate is the final check before opening a pull request. It runs the checks that I put in ship-manifest.md and then runs my pr-precheck. The part I had to decide was which tests or checks were important enough that they needed to pass before I asked somebody else to review the PR.
How it evolved, dated: In October 2026, during Unit 7, I installed ship-gate and created the manifest for my repository. This was different from the earlier tools because it connected several parts of my toolkit together. Instead of checking everything separately, I could run one gate before opening the PR.
Evidence: I could run the gate with:
python3 ~/.claude/ship-gate/check_gate.py --branch --title "<PR title>" --body-file pr_draft.md
The gate runs the checks in my manifest and then my pr-precheck. Depending on the branch, it can pass, block the PR and explain why, or refuse if something needed for the check is not ready.
- unit-1 harness: A tool from the beginning of the course that I used for testing and evaluation. It stayed part of my toolkit while I added the newer tools.
Beat 1
Date: October 2026
Friction: I needed a better way to decide which issues were actually realistic for me to work on instead of just picking one that looked interesting.
Change: I used issue-scout to organize the important information about each issue and added my own criteria for deciding if an issue was a good choice.
Result: I had a more organized way to decide what issue I wanted to work on.
Beat 2
Date: October 2026
Friction: After working on an issue, I did not have one clear way to decide whether the contribution was actually ready for a pull request.
Change: I created pr-precheck and used my own rubric to check the code changes, plan, testing evidence, and PR draft.
Result: I had a checklist that could point out things I still needed to fix before opening the PR.
Beat 3
Date: October 2026
Friction: Even if the code was ready, the PR description could still be unclear or not follow the writing style I wanted.
Change: I created a voice guide and used voice-guard to check the PR text.
Result: The writing became another part of the PR process instead of something I only checked at the very end.
Beat 4
Date: October 2026
Friction: I had several tools that worked separately, but I still had to remember to run everything before opening a pull request.
Change: I installed ship-gate and connected the repository checks, my pr-precheck, and the PR-opening process together.
Result: I had one final place where my work could be checked before I tried to open the pull request.
The tools are useful, but they still depend a lot on the rules and information I give them. If I write a bad rubric or leave an important check out of the manifest, the tool might not catch something that matters. I also think the tools could be easier to use together. Right now there are still different files and reports that I have to keep track of.
If I kept working on the toolkit, I would probably make the reports simpler and make it easier to see the whole process in one place. I would also keep adjusting the rules based on problems I actually run into instead of trying to predict every possible problem ahead of time.
Tool limits.
issue-scout: It only knows the information it can find about the issue and repository, so it can still miss context that a maintainer knows.
pr-precheck: It depends on the rubric I wrote, so it can miss something if my rubric does not cover it well.
voice-guard: It checks against my voice guide, but good writing can still depend on the specific repository and maintainer.
ship-gate: It depends on the manifest being set up correctly, and some checks might not be possible to run on my computer.
