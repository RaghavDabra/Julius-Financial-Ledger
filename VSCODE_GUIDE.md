# LedgerLens: step-by-step from VS Code to a live Vercel site

You need: VS Code, Git, a GitHub login with push access to MananDabra/General-Ledger-Financial-Modelling, and a Vercel account.

## 1. Download the patch
Download `vercel-deploy.patch` from this project (it's attached in the thread). Note where it lands, e.g. `~/Downloads/vercel-deploy.patch`.

## 2. Open the repo in VS Code
- **Already have it locally?** VS Code > File > Open Folder > pick the `General-Ledger-Financial-Modelling` folder.
- **Don't have it?** Press `Ctrl+Shift+P` (Mac: `Cmd+Shift+P`) > type `Git: Clone` > paste
  `https://github.com/MananDabra/General-Ledger-Financial-Modelling.git` > choose a folder > click **Open**.

## 3. Open the terminal
Menu: Terminal > New Terminal. It opens inside the repo folder.

## 4. Get the latest dev
```bash
git checkout dev
git pull
```
If it complains about uncommitted changes, commit or stash them first (`git stash`).

## 5. Apply the fix
Copy the patch into the repo folder (drag it into VS Code's file explorer), then:
```bash
git am vercel-deploy.patch
```
- If Git asks who you are: `git config user.name "Your Name"` and `git config user.email "you@example.com"`, then run `git am vercel-deploy.patch` again.
- If it says "patch does not apply": run `git am --abort`, then `git apply --3way vercel-deploy.patch`, then `git add -A && git commit -m "Make LedgerLens deployable on Vercel"`.

Delete the patch file afterwards so it doesn't get committed: `rm vercel-deploy.patch` (Windows PowerShell: `del vercel-deploy.patch`).

## 6. Check it builds (optional, about a minute)
```bash
npm install
npm run build
```
It should end with `built in ...s`. A "chunks are larger than 500 kB" warning is fine.

## 7. Push to GitHub
```bash
git push origin dev
```
Or click **Sync Changes** in VS Code's Source Control panel (the branch icon on the left).

## 8. Deploy on Vercel
1. Go to vercel.com > **Add New** > **Project**.
2. Import `General-Ledger-Financial-Modelling` (click "Adjust GitHub App Permissions" if you don't see it).
3. Leave Framework Preset, Build Command and Output Directory as they are.
4. Optional: under Environment Variables add `GEMINI_API_KEY` = your key. Without it the app still works, with templated AI text.
5. Click **Deploy** and wait for it to finish.

## 9. Test the live site
- `https://<your-app>.vercel.app/api/health` should show `"ok": true`.
- `https://<your-app>.vercel.app` should load with Deloitte plus 3 sample engagements in the sidebar.

If the deploy fails, copy the red error lines from the Vercel build log and paste them in the thread.

## Running it locally (optional)
Terminal 1: `pip install -r requirements.txt` then `cd api && python app.py` (Flask runs on port 8080)
Terminal 2: `npm run dev`, then open http://localhost:5173
