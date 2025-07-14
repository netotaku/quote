# 🔐 Codeword Puzzle Game

A Vue-powered browser game inspired by classic *codeword* puzzles — using motivational quotes from [ZenQuotes.io](https://zenquotes.io). Players decipher hidden letters using numeric clues, guessing the characters of the quote with a limited number of lives.

Hosted on [Netlify](https://www.netlify.com) with serverless functions and built-in mobile responsiveness.

---

## 🚀 Features

* ✍️ Inspirational quotes pulled from ZenQuotes via Netlify serverless function
* 🔠 Each letter replaced with a unique code
* 🎯 Players must guess missing letters — only a few are revealed at the start
* ❤️ 4 lives per game — incorrect guesses cost lives
* 🔁 “Next Quote” button for continuous play
* 📱 Mobile-friendly layout with focus looping for fast keyboard entry
* 🧠 Punctuation automatically displayed — only letters are guessable
* 🎨 Styled with scoped Sass and global helpers

---

## 🛠 Tech Stack

* [Vue 3](https://vuejs.org/)
* [Vite](https://vitejs.dev/)
* [Sass](https://sass-lang.com/)
* [Netlify CLI](https://docs.netlify.com/cli/get-started/) (for local dev + serverless functions)
* [ZenQuotes API](https://zenquotes.io/api)

---

## 📦 Installation

```bash
git clone https://github.com/netotaku/quote
cd quote
npm install
```

---

## 🧪 Running Locally

Make sure the [Netlify CLI](https://docs.netlify.com/cli/get-started/) is installed globally:

```bash
npm install -g netlify-cli
```

Then link to your Netlify site (first time only):

```bash
netlify link
```

Start the dev server:

```bash
netlify dev
```

* Runs Vite dev server
* Serves Netlify Functions locally from `/netlify/functions/`
* Accessible at [http://localhost:8888](http://localhost:8888)

---

## 📁 Project Structure

```bash
src/
├── components/
│   ├── Quote.vue        # Main logic
│   ├── Word.vue         # Renders individual words
│   └── Letter.vue       # Handles user input per letter
├── assets/
│   └── styles.scss      # Global styles
├── App.vue
└── main.js

netlify/
└── functions/
    └── quote.js         # Serverless proxy to ZenQuotes API
```

---

## 📱 iOS Considerations

* Disabled inputs are styled white with `-webkit-text-fill-color` for Safari
* `autocapitalize="off"` and `spellcheck="false"` used to prevent unwanted keyboard behaviours
* Focus loop implemented to jump between editable fields
* `position: fixed` adjusted to behave correctly when keyboard is open

---

## 🧠 Known Improvements (Wishlist)

* Add difficulty modes (e.g. fewer starter letters, timed mode)

---

## 📜 License

MIT — do what you want, just don’t pass it off as your own without credit.

---

## 🙌 Credits

Built by Nick C using Vue, ZenQuotes, and a whole lot of thoughtful iteration.

Game logic, UX flow, and component design developed incrementally for clarity and control.

---

### One-liner summary

A collaborative build of a Vue-powered codeword game that blends thoughtful UX, real-time gameplay, and AI-assisted development.

### Hire me

If you're looking for someone who can blend UX, development, and AI tooling into smart, efficient digital builds — I’m available for freelance and consultancy work.
