# Day 27: Git, Version Control & Collaboration Practices

**Week 4 · Systems, Design & Practice**

## 🎯 Learning Objectives
- Use Git confidently beyond add/commit/push
- Resolve merge conflicts and understand rebasing vs merging
- Follow professional collaboration practices (branching, code review)

## 📚 Topics
- Git internals at a conceptual level (commits, branches, HEAD)
- Merging vs rebasing
- Resolving merge conflicts
- Branching strategies & code review etiquette

## 💡 Core Ideas
- A Git commit is a snapshot with a pointer to its parent(s) — branches are just movable pointers to commits, which is why branching is so cheap.
- Merging preserves history exactly as it happened (with a merge commit); rebasing rewrites history to look linear — know which your team prefers and why.
- A good code review comment explains the *why*, not just the *what* — this is a communication skill as much as a technical one.

## 🛠️ Practice Exercises
- [ ] Create a repo, make two branches that edit the same line of a file, merge them, and manually resolve the conflict.
- [ ] Practice an interactive rebase: squash 3 commits into 1 with a clean commit message.
- [ ] Write a code review comment (on your own or a sample PR) that explains reasoning, not just 'change this'.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- What's the practical difference between `git merge` and `git rebase`, and when would you use each?
- What does `git reset --hard` actually do, and why is it dangerous on a shared branch?
- What makes a good commit message, and why does it matter months later?

## 🔗 Resources
- https://learngitbranching.js.org/
- https://www.atlassian.com/git/tutorials

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-26.md) | [🏠 Home](README.md) | [Next Day ➡](day-28.md)
