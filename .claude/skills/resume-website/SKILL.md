---
name: resume-website
description: Build a stunning personal website from a resume PDF. Use this skill whenever the user wants to create a personal website, portfolio site, or online resume from a PDF resume/CV. Also trigger when the user mentions converting a resume to a website, building a portfolio from their CV, or wants a personal landing page based on their professional background.
---

# Resume Website Builder

Build a distinctive, production-grade personal website from a resume PDF. The website should reflect the user's profession, personality, and design preferences — not look like a generic template.

## Workflow

### Step 1: Ensure PDF Reader MCP is Available

Before reading any PDF, verify the pdf-reader MCP is installed. If not, install it:

```bash
claude mcp add pdf-reader -- npx @sylphx/pdf-reader-mcp
```

Then use the MCP's `read_pdf` tool to extract the resume content.

### Step 2: Read and Analyze the Resume

Use the pdf-reader MCP to read the user's resume PDF. Extract and organize:

- **Name and contact info** (email, phone, location, LinkedIn, GitHub, portfolio links)
- **Professional title / headline**
- **Work experience** (companies, roles, dates, achievements)
- **Education** (degrees, institutions, dates)
- **Skills** (technical, soft, languages)
- **Projects** (if any)
- **Certifications, awards, publications** (if any)
- **Summary / objective** (if any)

### Step 3: Analyze the User's Profession

The user's job determines the website's aesthetic direction. Match the design to their professional identity:

| Profession Category | Suggested Direction |
|---|---|
| Software Engineer / Developer | Clean, technical, dark themes, monospace accents, terminal aesthetics, code-inspired |
| Designer / Creative Director | Bold, experimental, maximalist or refined minimal, strong typography |
| Data Scientist / Analyst | Data-viz inspired, structured grids, charts as decoration, precision |
| Marketing / Business | Polished, confident, editorial magazine feel, strong CTAs |
| Academic / Researcher | Scholarly, serif-heavy, paper-like, intellectual minimalism |
| Healthcare / Medical | Clean, trustworthy, calming colors, accessibility-first |
| Finance / Consulting | Luxury, refined, dark + gold accents, authority |
| Artist / Photographer | Gallery-like, image-forward, dramatic spacing |
| Product Manager | Modern SaaS aesthetic, organized, metric-driven |
| Teacher / Educator | Warm, approachable, friendly colors, readable |
| Legal | Traditional, authoritative, serif fonts, structured |
| Freelancer / Generalist | Personal brand focused, unique personality expression |

Use this as a starting point, but adapt based on the specific details in the resume.

### Step 4: Ask the User's Design Preferences

Use the `AskUserQuestion` tool to gather design preferences. Ask questions in a friendly, non-technical way since users may not know design terminology. Present clear options they can choose from.

**Question categories to ask (generate options dynamically based on the user's profession, industry, and resume content):**

After analyzing the resume in Steps 2-3, generate 3-5 tailored options per category that make sense for this specific person. For example, a software engineer should see options like "terminal/CLI aesthetic" and "dark mode with syntax-highlighting accents", while a fashion designer should see "editorial lookbook" and "high-contrast monochrome with bold typography". Always include a "Something else" and a "Surprise me" escape hatch.

The categories to cover:

1. **Overall vibe** — Suggest 3-4 aesthetic directions that fit their profession. Lead with your top recommendation and explain why it suits them. Each option should include a brief plain-language description of what it looks and feels like.

2. **Color preference** — Offer 4-5 color palettes curated for their industry. Describe each with mood words, not just color names (e.g., "deep navy + copper accents — feels authoritative and warm" rather than just "dark blue and orange"). Always include an option for the user to specify their own colors.

3. **Visual style** — Present 3-4 design styles relevant to their field. Only show styles that actually make sense — a lawyer probably shouldn't see "brutalism" as the top pick, a creative coder probably shouldn't see "traditional corporate". Briefly explain each with a one-line "this feels like..." description.

4. **Typography feel** — Suggest 3-4 font personality directions that match the vibe. Describe the feeling each conveys rather than naming specific fonts (e.g., "sharp geometric sans-serif — modern, precise, tech-forward" not "use Inter").

5. **Content layout** — Single page vs. multi-page, but frame the recommendation based on how much content their resume has. If they have 10+ years of experience and many projects, lean toward multi-page. If it's concise, suggest single page.

6. **Photos/media** — Ask what assets they can provide (profile photo, project screenshots, videos, logos). Adapt suggestions to their role — ask a photographer about gallery images, ask a developer about project demos. **Explicitly prompt them to drop any photos or videos into the repo now** (before building) so you can reference them — e.g., "If you have a profile photo or any project screenshots/videos you'd like on the site, add them to the repo and let me know the filenames."

7. **Animation & interaction level** — Offer 3-4 levels of motion, but tailor the descriptions to their field. A creative director might see "cinematic page transitions with scroll-triggered reveals", while an accountant might see "clean fade-ins that keep the focus on content".

8. **Background & texture** — Suggest 3-4 background treatments that complement the visual styles offered above. These should feel cohesive with the other choices, not random.

9. **Special features** — Curate a list of 4-6 extras that are relevant to their profession. A developer might see "GitHub contribution graph widget" and "live project demo embeds"; a consultant might see "client logo carousel" and "case study cards". Always include basics like "downloadable PDF resume" and "contact section".

**How to ask:** You MUST use `AskUserQuestion` to cover **all 9 categories** — do not skip any. Batch related categories together (e.g., ask about vibe + color palette in one question, visual style + typography in another, animation + background in another, and content layout + photos/media + special features in another) to keep the conversation flowing without feeling like a long form. Keep the tone conversational and non-technical. After each answer, you may ask a brief follow-up to refine the choice (e.g., if they pick a dark theme, ask "do you want pure black or a softer dark like charcoal/navy?"). The goal is to give users choices they didn't know existed while keeping it feeling easy, not like a quiz.

**Required coverage checklist — confirm all 9 are answered before moving to Step 5:**
- [ ] 1. Overall vibe
- [ ] 2. Color preference
- [ ] 3. Visual style
- [ ] 4. Typography feel
- [ ] 5. Content layout
- [ ] 6. Photos/media (and prompt to provide files)
- [ ] 7. Animation & interaction level
- [ ] 8. Background & texture
- [ ] 9. Special features

### Step 5: Generate the Design System

Use the `frontend-design` skill's to generate a design system based on the user's profession and preferences:


### Step 6: Build the Website

Create the static HTML website in the current working directory. Follow these principles:

**Technical Requirements:**
- Pure static HTML + CSS + JS (no build tools, no frameworks)
- All styles inline or in `<style>` tags (single file preferred, but can split into multiple pages)
- Use Google Fonts via CDN link for typography
- Use SVG icons (Heroicons, Lucide, or custom) — NEVER use emojis as icons
- Responsive design — must look great on mobile, tablet, and desktop
- Accessible — proper semantic HTML, alt text, contrast ratios, keyboard navigation

**Content Mapping from Resume:**
- Hero section with name, title, and a compelling one-liner
- About section with professional summary
- Experience section with timeline or cards
- Skills section with visual representation (bars, tags, or grouped categories)
- Education section
- Projects section (if applicable)
- Contact section with links and optional form
- Footer with social links

**Design Execution:**
- Follow the frontend-design skill principles: be bold, distinctive, and avoid generic AI aesthetics
- Match the design to the user's profession (see Step 3)
- Apply the design system from Step 5
- Add thoughtful animations and micro-interactions (CSS-only preferred)
- Use CSS custom properties for consistent theming
- Create visual hierarchy — the most important info should stand out

**File Structure:**
- Save the main file as `index.html` in the current directory
- If multi-page: create additional HTML files (e.g., `about.html`, `projects.html`)
- If user provides assets: save them in an `assets/` subdirectory
- Add a `style.css` only if styles are too large for inline

### Step 7: Review and Iterate

After building, tell the user:
- What you built and the design decisions you made
- How to view it (open `index.html` in a browser)
- Ask if they want any changes — colors, layout, content, animations, etc.

Be ready to iterate based on feedback.
