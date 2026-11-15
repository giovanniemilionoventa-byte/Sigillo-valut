# Sigillo: where things stand (November 2026)

Hi Polina, here is a short summary of Sigillo as it is now. It will stay like this for a while: no changes are planned.

## What Sigillo does
- It keeps a **diary of everything an AI agent does**: every question to the AI model, every tool it uses, every decision.
- Every line of the diary is **sealed**, so nobody can change or delete it afterwards without it showing. Not the client, and not us.
- Every hour the diary gets a **time stamp from an outside company**, which proves when each entry existed.
- The client can **download a proof file** and give it to an auditor. The auditor checks it with a free program, without having to trust us.
- **We never see the client's texts.** The diary keeps only a kind of "fingerprint" of each text, not the text itself. The key that could unlock it stays on the client's computer.
- If the internet goes down, **nothing is lost**: the agent keeps working, and the diary catches up by itself when the internet is back.
- **The agent is never stopped** by Sigillo.

## What Sigillo does not do (say this honestly)
- It does **not stop** an agent from doing something wrong. It records; it does not control.
- It **cannot prove the agent recorded everything**. If a programmer deliberately removes Sigillo from the agent, actions after that are missing. We do see when the agent goes silent, and when its code is changed.
- The time stamp is **not the "qualified" legal kind** (eIDAS). Banks may ask for that later.
- It is **not a law-compliance certificate**. Never say "compliant" or "tamper-proof". Say "tamper-evident": any change shows.

## What to tell clients, in one sentence
"Sigillo is a sealed diary of your AI agent: it proves to any auditor what the agent did and when, without us ever reading your data."

## Who it is for
Software companies that build AI agents in Python, starting in Italy and Spain; banks and insurers later.
