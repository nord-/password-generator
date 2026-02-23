# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

XKCD-style password generator ("Lösenordsgenerator") — a standalone single-file web application (`password-generator.html`) with no build system, dependencies, or server required. Open directly in a browser.

## Architecture

Everything lives in one HTML file with embedded `<style>` and `<script>` sections:

- **CSS** (lines 8–319): Dark-themed UI using CSS custom properties (`:root` vars). Fonts: JetBrains Mono (passwords/headings) + DM Sans (UI text), loaded from Google Fonts.
- **JavaScript** (lines 349–932): Vanilla JS, no frameworks.

### Key data structures
- `EN_WORDS` (~1000 English words) and `SV_WORDS` (~500 Swedish words) — combined into `ALL_WORDS` for generation
- `LEET` — character substitution map (a→4, e→3, i→1, o→0, s→5, t→7, l→1, b→8, g→9)
- `SEPARATORS` — hyphen, space, slash, asterisk, underscore

### Password generation flow (`generatePassword`)
1. Pick a random separator
2. Choose 3 or 4 random words from the combined word list
3. Apply random case transforms (lowercase/uppercase/title case)
4. Ensure at least one lowercase and one uppercase letter
5. Apply leet-speak substitution to one random character in one random word
6. Retry up to 200 times until the result fits min/max length constraints
7. Return both plain text and HTML (with `.leet` and `.sep` spans for styling)

`generateAll()` produces 4 passwords at once with entropy display. Entropy is calculated as `length × log₂(charset_size)` with color-coded indicators (green ≥80 bits, yellow ≥60, orange <60).

### UI language
All user-facing text is in **Swedish**. Maintain this convention.

## Development

No build, lint, or test commands — just open `password-generator.html` in a browser. When making changes, keep everything in the single file to preserve the zero-dependency nature of the project.
