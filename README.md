# 🔍 GitHub Contributors by Location (Bookmarklet)

This simple bookmarklet allows you to quickly find GitHub contributors to any repository, filtered by location, right from your browser!

## 📌 Features

- **Instantly fetch contributors** based on owner, repo, and location.
- **Remember your GitHub token** securely in your browser.
- **Clickable links** directly to contributor profiles.
- Clean, readable overlay interface.

---

## 🚀 Bookmarklet Installation

**Step 1:** Create a new bookmark in your browser's bookmarks bar.

**Step 2:** Name it `GitHub Contributors by Location` (or any preferred name).

**Step 3:** Copy the code snippet below and paste it into the URL field of your bookmark:

```javascript
javascript:(async function(){if(document.getElementById('gh_contrib_modal'))return;const savedToken=localStorage.getItem('gh_token');let token;if(savedToken){token=savedToken;}else{token=prompt('Enter GitHub Token (saved locally):');if(token===null)return;localStorage.setItem('gh_token',token);}const headers=token?{Authorization:`token ${token}`}:{};const owner=prompt('GitHub Owner:');if(!owner)return;const repo=prompt('GitHub Repo:');if(!repo)return;const locFilter=prompt('Filter by Location:');if(locFilter===null)return;const style=document.createElement('style');style.textContent=`#gh_contrib_modal{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.75);display:flex;align-items:center;justify-content:center;z-index:999999;font-family:Arial,sans-serif;}#gh_contrib_box{position:relative;background:#fff;padding:20px;width:80%;max-width:700px;max-height:70%;overflow:auto;border-radius:10px;box-shadow:0 4px 12px rgba(0,0,0,0.3);color:#333;line-height:1.5;}#gh_contrib_close{position:absolute;top:10px;right:15px;cursor:pointer;font-size:20px;color:#888;}#gh_contrib_close:hover{color:#000;}#clear_token_btn{margin-top:10px;font-size:12px;color:#888;cursor:pointer;}#clear_token_btn:hover{color:red;}h3{margin-top:0;}a{text-decoration:none;color:#0366d6;}a:hover{text-decoration:underline;}#gh_contrib_results div{margin-bottom:8px;padding-bottom:8px;border-bottom:1px solid #eee;}`;document.head.appendChild(style);const modal=document.createElement('div');modal.id='gh_contrib_modal';modal.innerHTML=`<div id="gh_contrib_box"><div id="gh_contrib_close">✖️</div><h3>GitHub Contributors (${locFilter})</h3><div id="gh_contrib_results">Loading contributors...</div><div id="clear_token_btn">🗑️ Clear saved token</div></div>`;document.body.appendChild(modal);document.getElementById('gh_contrib_close').onclick=()=>{document.body.removeChild(modal);document.head.removeChild(style);};document.getElementById('clear_token_btn').onclick=()=>{localStorage.removeItem('gh_token');alert('GitHub token removed from local storage.');};async function fetchJson(url){const res=await fetch(url,{headers});if(!res.ok)throw new Error(`HTTP ${res.status}: ${res.statusText}`);return res.json();}try{let page=1,hasMore=true,results=[];while(hasMore&&page<=3){const contribs=await fetchJson(`https://api.github.com/repos/${owner}/${repo}/contributors?per_page=100&page=${page}`);if(contribs.length===0){hasMore=false;break;}for(const user of contribs){const u=await fetchJson(user.url);if(u.location&&u.location.toLowerCase().includes(locFilter.toLowerCase())){results.push(`<div><a href="${u.html_url}" target="_blank"><strong>${user.login}</strong></a> (${u.location}) – ${user.contributions} contributions</div>`);}await new Promise(r=>setTimeout(r,150));}page++;}document.getElementById('gh_contrib_results').innerHTML=results.length?results.join(''):`No contributors found from "${locFilter}".`;}catch(e){document.getElementById('gh_contrib_results').innerHTML=`Error: ${e.message}`;}})();
```

---

## 🎯 How to Use

1. Click your newly created bookmarklet in the bookmarks bar.
2. **First time use:** You'll be asked for your GitHub token. It's saved locally, so you don't need to enter it again.
3. Type the GitHub repository **Owner**, **Repo**, and desired **Location**.
4. Results instantly appear as a convenient overlay.
5. Click contributor usernames to open their GitHub profiles in a new tab.

---

### 🗑️ Clearing Your Saved GitHub Token

- In the results overlay, click the `🗑️ Clear saved token` link to remove your GitHub token from local storage. You'll be prompted for a new token on your next use.

---

## 🔐 How to Get a GitHub Token

To avoid hitting API rate limits and to access more contributors, it’s recommended to use a **GitHub Personal Access Token (PAT)**.

### Steps:
1. Go to [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **"Generate new token"** (classic).
3. Give your token a name (e.g., `Bookmarklet Use`).
4. Select **only** the `public_repo` permission (this is enough for public repos).
5. Click **Generate token**.
6. Copy the token and **store it securely** (you’ll only see it once).

> ⚠️ This token is saved only in your browser's local storage and never leaves your machine.

---

## 📽️ Demo Video

You can see how the bookmarklet works in this short demo:

👉 <a href="https://youtube.com/shorts/Lghmm4tjRTo" target="_blank">Watch the YouTube video</a>

Or embed it directly (if supported by your markdown viewer):

```html
<video src="https://www.youtube.com/embed/Lghmm4tjRTo" controls width="700"></video>
```

---

**Happy Exploring! 🎉**

