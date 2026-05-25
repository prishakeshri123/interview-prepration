# 📖 1.2: HTML — The Skeleton
## Complete Mastery Guide: Teach + Interview + ELI5 + Quiz + Challenge

---

## 🎯 PART 0: 30-Second Rule 0 Explanation

### The Concept in 30 Seconds

HTML is the skeleton of a website—it's the **markup language that structures all content** on a webpage. While CSS makes things look pretty and JavaScript makes them interactive, HTML is what organizes everything into a meaningful structure. Think of it as the blueprint: HTML defines what things ARE (a heading is a heading, this is a list, that's a form), and other technologies decide how they look and behave.

### Real-World Why This Matters

Every website you visit—Google, Netflix, Twitter—has HTML as its foundation. When you're building for billions of users (like these companies do), HTML architecture matters because:

- **Accessibility**: Screen reader users depend on proper HTML structure to navigate. Bad HTML = broken for 15% of users
- **SEO**: Google crawls HTML to understand your content. Semantic HTML = better rankings = more traffic
- **Performance**: Well-structured HTML loads faster and requires less JavaScript fixes
- **Maintainability**: Clean HTML makes it 10x easier for teams to work together

At Google, when engineers build Gmail or Google Drive, they obsess over semantic HTML because one simple `<div>` where a `<button>` should go can break keyboard navigation for millions of users.

### Production Impact

In production systems:
- **Accessibility lawsuits**: Companies pay millions for inaccessible websites (Domino's Pizza paid $3M in 2024)
- **SEO ranking drops**: Improper HTML structure = 40% traffic loss (happened to major retailers)
- **Mobile user frustration**: Unstructured HTML breaks on mobile devices (hard to tap buttons, confusing layout)
- **Maintenance costs**: Fixing broken HTML structure costs 5x more than building it right initially

---

## 📚 PART 1: TEACH - Deep Dive Learning

### What Is HTML?

HTML stands for **HyperText Markup Language**. It's not a programming language (doesn't have logic), it's a **markup language**—a system of labels (tags) that describe content.

**Brief History:**
- 1989: Tim Berners-Lee invented HTML with just 18 tags for basic documents
- 1995-2000: HTML grew chaotically; browsers invented proprietary tags
- 2014: HTML5 standardized everything; added semantic meaning and APIs
- Today: HTML5 + modern CSS/JS = complete web platform

**Key Point**: HTML is about *meaning*, not appearance. A `<h1>` means "this is the main heading"—not "make it big and bold." The big and bold part comes from CSS.

### Core Concepts

#### 1. Semantic HTML5 Elements (THE MOST IMPORTANT)

Semantic HTML means using tags that have **meaning**—they tell the browser (and assistive devices) what the content IS, not just how it looks.

**Why Semantic HTML Matters:**
- Screen readers announce "main navigation" instead of "clickable box"
- Search engines understand your page structure
- Developers understand code faster (clearer intent)
- Browsers can render more efficiently

```html
<!-- ❌ BAD: All divs (meaningless) -->
<div class="header">
  <div class="nav">
    <div class="link">Home</div>
    <div class="link">About</div>
  </div>
</div>
<div class="main-content">
  <div class="article">
    <div class="title">Article Title</div>
    <div class="body">Content here...</div>
  </div>
</div>
<div class="footer">
  <div class="text">Copyright</div>
</div>

<!-- ✅ GOOD: Semantic structure (meaningful) -->
<header>
  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>
<main>
  <article>
    <h1>Article Title</h1>
    <p>Content here...</p>
  </article>
</main>
<footer>
  <p>Copyright 2024</p>
</footer>
```

**Key Semantic Elements:**

| Element | Purpose | When to Use |
|---------|---------|------------|
| `<header>` | Top section (logo, nav, intro) | Site/page introduction |
| `<nav>` | Navigation links | Primary navigation only |
| `<main>` | Main content area | One per page, unique content |
| `<article>` | Self-contained content | Blog posts, news articles, comments |
| `<section>` | Thematic grouping | Group related content |
| `<aside>` | Sidebar/supplementary | Tangential to main content |
| `<footer>` | Bottom section (copyright, links) | Page/site footer |

**Real-World Example - Netflix's HTML:**
```html
<!-- Netflix uses semantic structure -->
<header>
  <nav>
    <!-- Logo, user menu, search -->
  </nav>
</header>

<main>
  <section class="hero">
    <!-- Featured show/movie -->
  </section>
  
  <section class="recommendations">
    <!-- Different categories of shows -->
  </section>
</main>

<footer>
  <!-- Links, company info -->
</footer>
```

Netflix's semantic structure helps:
- Screen reader users navigate categories
- Google understand what content is on the page
- Developers maintain the codebase (clear structure)

#### 2. Forms & Validation (CRITICAL FOR INTERVIEWS)

Forms are where user input happens. Getting HTML forms right is CRUCIAL because:
- 80% of web interactions are forms (login, checkout, signup)
- Poor form UX = users abandon before finishing
- Validation prevents bad data and security issues

```html
<!-- ❌ BAD: No structure, no validation -->
<input placeholder="Email">
<input placeholder="Password">
<button>Login</button>

<!-- ✅ GOOD: Proper form structure with validation -->
<form>
  <label for="email">Email</label>
  <input 
    id="email" 
    name="email" 
    type="email" 
    required 
    aria-label="Email address"
  >
  
  <label for="password">Password</label>
  <input 
    id="password" 
    name="password" 
    type="password" 
    required 
    minlength="8" 
    aria-label="Password"
  >
  
  <button type="submit">Login</button>
</form>
```

**Key Form Attributes:**

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `type` | Input type validation | `type="email"`, `type="number"` |
| `required` | Field must be filled | `<input required>` |
| `pattern` | Regex validation | `pattern="[0-9]{3}-[0-9]{4}"` |
| `minlength` / `maxlength` | String length | `minlength="8"` |
| `min` / `max` | Numeric range | `min="18"` `max="100"` |
| `step` | Number increments | `step="0.01"` for prices |

**HTML5 Input Types (INTERVIEW QUESTION):**
```html
<!-- Email with browser validation -->
<input type="email" required>

<!-- Password (masked characters) -->
<input type="password" required>

<!-- Number with validation -->
<input type="number" min="1" max="100">

<!-- Date picker -->
<input type="date">

<!-- Color picker -->
<input type="color">

<!-- File upload -->
<input type="file" accept=".pdf,.doc">

<!-- Search with browser search UI -->
<input type="search">

<!-- URL validation -->
<input type="url" required>
```

**Custom Validation (JavaScript + HTML5):**
```html
<form id="form">
  <input 
    id="password" 
    type="password" 
    pattern="^(?=.*[A-Z])(?=.*\d).{8,}$"
  >
  <span id="error"></span>
  <button type="submit">Submit</button>
</form>

<script>
// Custom validation beyond HTML5
const input = document.getElementById('password');

// Constraint validation API
input.addEventListener('invalid', (e) => {
  if (e.target.validity.patternMismatch) {
    e.target.setCustomValidity(
      'Password needs 8+ chars, 1 uppercase, 1 number'
    );
  }
});

// Also check on input
input.addEventListener('input', (e) => {
  e.target.setCustomValidity(''); // Reset
});
</script>
```

#### 3. Accessibility (ARIA, Labels, Landmarks)

Accessibility isn't a feature—it's a **fundamental requirement**. 

**The Impact:**
- 15% of global population has disabilities
- In enterprise: 25% of employees have accessibility needs
- Legal requirement in USA (ADA), EU (AODA), UK (Equality Act)
- Companies pay millions in lawsuits (Domino's, Amazon, Target)

**Key Accessibility Concepts:**

**a) Proper Labels (CRITICAL):**
```html
<!-- ❌ BAD: No label connection -->
<input type="text" placeholder="Enter name">

<!-- ✅ GOOD: Label linked to input -->
<label for="name">Name</label>
<input id="name" type="text">

<!-- ✅ ALSO GOOD: Label wraps input -->
<label>
  Name
  <input type="text">
</label>
```

**b) ARIA Roles & Attributes:**
```html
<!-- ARIA = Accessible Rich Internet Applications -->
<!-- Used when semantic HTML isn't available -->

<!-- Custom button (not <button> tag) -->
<div role="button" tabindex="0" aria-label="Close menu">
  ✕
</div>

<!-- Alert that updates dynamically -->
<div role="alert" aria-live="assertive">
  Error: Email already exists
</div>

<!-- Dropdown menu -->
<div role="menu">
  <button role="menuitem">Edit</button>
  <button role="menuitem">Delete</button>
</div>

<!-- Expanded menu indicator -->
<button 
  aria-expanded="false" 
  aria-controls="menu"
  onclick="toggleMenu()"
>
  ☰ Menu
</button>

<!-- Skip to main content (SEO + accessibility) -->
<a href="#main" class="skip-link">Skip to main content</a>
<main id="main">...</main>
```

**c) Keyboard Navigation:**
```html
<!-- All interactive elements must be keyboard accessible -->

<!-- Keyboard-accessible custom dropdown -->
<div role="combobox" aria-expanded="false">
  <input 
    role="searchbox"
    aria-autocomplete="list"
    onkeydown="handleArrowKeys(event)"
  >
  <ul role="listbox">
    <li role="option">Option 1</li>
    <li role="option">Option 2</li>
  </ul>
</div>

<!-- tabindex for custom focus order -->
<button tabindex="0">First focus</button>
<button tabindex="1">Second focus</button>

<!-- Remove from tab order if needed -->
<div tabindex="-1" aria-hidden="true">Hidden content</div>
```

**Common Accessibility Gotchas:**
- ❌ Using `<div onclick>` instead of `<button>` (not keyboard accessible)
- ❌ No labels on form fields (screen readers can't find them)
- ❌ Images without alt text (screen readers see nothing)
- ❌ Color-only information (colorblind users miss it)
- ❌ Auto-playing audio/video (startling for screen reader users)
- ❌ Keyboard trap (focus gets stuck, can't escape)

#### 4. SEO Fundamentals

SEO is how people find your website through Google search. From an engineer's perspective, SEO is about **helping Google understand your content**.

**Meta Tags:**
```html
<head>
  <!-- Page title (shown in Google results) -->
  <title>Best Pizza in New York | Tony's Pizzeria</title>
  
  <!-- Description (shown under title in Google) -->
  <meta name="description" 
        content="Authentic New York pizza since 1985. Award-winning recipes. Open 11am-11pm daily.">
  
  <!-- Viewport (mobile responsive) -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- Character encoding (should be UTF-8) -->
  <meta charset="UTF-8">
  
  <!-- Open Graph (social media previews) -->
  <meta property="og:title" content="Best Pizza in New York | Tony's Pizzeria">
  <meta property="og:description" content="Authentic NY pizza since 1985">
  <meta property="og:image" content="https://example.com/pizza.jpg">
  <meta property="og:url" content="https://example.com">
  <meta property="og:type" content="website">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Best Pizza in New York">
  <meta name="twitter:image" content="https://example.com/pizza.jpg">
</head>
```

**Structured Data (JSON-LD):**
```html
<!-- Structured data helps Google understand content type -->

<!-- Schema for local business -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Tony's Pizzeria",
  "image": "https://example.com/pizza.jpg",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main St",
    "addressLocality": "New York",
    "postalCode": "10001"
  },
  "telephone": "(212) 555-1234",
  "url": "https://example.com",
  "openingHoursSpecification": {
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": "Monday",
    "opens": "11:00",
    "closes": "23:00"
  }
}
</script>
```

#### 5. Web Components Basics

Web Components let you create **custom reusable HTML elements**.

```html
<!-- Define custom element -->
<script>
class CustomButton extends HTMLElement {
  constructor() {
    super();
    // Shadow DOM = isolated styles/structure
    this.attachShadow({ mode: 'open' });
  }
  
  connectedCallback() {
    this.shadowRoot.innerHTML = `
      <style>
        button {
          background: #007bff;
          color: white;
          padding: 10px 20px;
          border: none;
          border-radius: 4px;
        }
      </style>
      <button><slot></slot></button>
    `;
  }
}

customElements.define('custom-button', CustomButton);
</script>

<!-- Use custom element -->
<custom-button>Click Me</custom-button>
```

**Key Concepts:**
- **Shadow DOM**: Encapsulated styles (CSS in shadow DOM doesn't affect outside)
- **Slots**: Placeholder for custom content
- **Custom Elements**: Define your own HTML tags

#### 6. Canvas & SVG Basics

Two ways to draw graphics in HTML:

**SVG (Scalable Vector Graphics):**
```html
<!-- SVG = Vector graphics (scales perfectly, always crisp) -->
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg">
  <!-- Circle -->
  <circle cx="100" cy="100" r="80" fill="blue" />
  
  <!-- Rectangle -->
  <rect x="10" y="10" width="100" height="50" fill="red" />
  
  <!-- Text -->
  <text x="100" y="100" text-anchor="middle" fill="white">
    Hello SVG
  </text>
  
  <!-- Path (complex shape) -->
  <path d="M 10 10 L 100 100 L 10 100 Z" fill="green" />
</svg>
```

**Canvas (Bitmap Graphics):**
```html
<!-- Canvas = Raster graphics (fixed pixels, imperative drawing) -->
<canvas id="canvas" width="200" height="200"></canvas>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

// Draw circle
ctx.beginPath();
ctx.arc(100, 100, 50, 0, Math.PI * 2);
ctx.fillStyle = 'blue';
ctx.fill();

// Draw text
ctx.fillStyle = 'white';
ctx.font = '20px Arial';
ctx.fillText('Hello Canvas', 50, 105);
</script>
```

**SVG vs Canvas:**
| SVG | Canvas |
|-----|--------|
| Vector (scalable) | Raster (fixed pixels) |
| DOM elements (can style with CSS) | Single element (imperative) |
| Great for logos, icons, diagrams | Great for games, charts, real-time graphics |
| Slower for complex scenes | Faster for many objects |
| Text searchable by Google | Text not searchable |

#### 7. HTML Document Structure

A complete HTML document has this structure:

```html
<!DOCTYPE html>
<!-- Doctype tells browser this is HTML5 -->

<html lang="en">
<!-- Root element, lang for accessibility -->

<head>
  <!-- Meta information (not visible on page) -->
  <meta charset="UTF-8">
  <!-- Character encoding: MUST be first in head -->
  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- Mobile responsive -->
  
  <title>Page Title</title>
  <!-- Browser tab title, Google result title -->
  
  <link rel="stylesheet" href="styles.css">
  <!-- External CSS -->
  
  <script src="script.js" defer></script>
  <!-- External JavaScript, defer = load after HTML -->
</head>

<body>
  <!-- Visible content -->
  
  <header>
    <!-- Top of page: logo, nav -->
  </header>
  
  <main>
    <!-- Main content (one per page) -->
  </main>
  
  <footer>
    <!-- Bottom of page: copyright, links -->
  </footer>
</body>

</html>
```

**Common Meta Tags (INTERVIEW CRITICAL):**
```html
<head>
  <!-- Charset (MUST be first) -->
  <meta charset="UTF-8">
  
  <!-- Viewport (mobile responsive) -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- IE compatibility -->
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  
  <!-- Favicon -->
  <link rel="icon" href="/favicon.ico">
  
  <!-- Theme color (mobile browser) -->
  <meta name="theme-color" content="#007bff">
  
  <!-- Apple touch icon -->
  <link rel="apple-touch-icon" href="/apple-icon.png">
</head>
```

### Visual Explanations

#### HTML Structure Hierarchy
```
┌─────────────────────────────────────────────────┐
│  <header> - Logo, Navigation                    │
├─────────────────────────────────────────────────┤
│  <main>                                         │
│  ┌─────────────────────────────────────────────┤
│  │ <article>                                   │
│  │ ┌──────────────────────────────────────────┤
│  │ │ <h1>Article Title</h1>                   │
│  │ │ <p>Content...</p>                        │
│  │ └──────────────────────────────────────────┤
│  │ <section class="related">                  │
│  │  ┌─────────────────────────────────────────┤
│  │  │ <h2>Related Articles</h2>               │
│  │  └─────────────────────────────────────────┤
│  └─────────────────────────────────────────────┤
│  <aside> - Sidebar                             │
└─────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────┐
│  <footer> - Copyright, Links                    │
└─────────────────────────────────────────────────┘
```

#### Semantic Elements vs Divs
```
SEMANTIC (✅ Better)
<header><nav><a>...</a></nav></header>
<main><article>...</article></main>
<footer>...</footer>

NON-SEMANTIC (❌ Worse)
<div class="header"><div class="nav"><div class="link">...</div></div></div>
<div class="main"><div class="article">...</div></div>
<div class="footer">...</div>

Why semantic wins:
- Search engines understand structure
- Screen readers announce sections properly
- Developers understand code intent
- Styling cascade works better
```

### Real-World Applications

#### Google Search (How it Works)
Google crawls your HTML to understand content:

1. **Finds `<title>`** → Uses as search result title
2. **Finds `<meta description>`** → Uses as snippet
3. **Finds semantic tags** → Understands page structure
4. **Finds `<h1>`, `<h2>`** → Extracts main topics
5. **Finds structured data** → Understands product, recipe, etc.
6. **Finds images with alt** → Indexes images
7. **Finds links** → Crawls to find more pages

```html
<!-- Good for Google -->
<article>
  <h1>How to Make Perfect Coffee</h1>
  <meta name="description" content="Step-by-step guide to brewing perfect coffee at home.">
  <p>Making great coffee requires...</p>
  <img src="coffee.jpg" alt="Perfect pour-over coffee">
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "HowTo",
    "name": "How to Make Perfect Coffee",
    "step": [...]
  }
  </script>
</article>
```

#### Netflix (Accessibility + Performance)
Netflix uses semantic HTML to:
- Help blind users navigate shows
- Load page structure faster (before CSS/JS)
- Improve SEO for show discoverability

```html
<main>
  <section aria-label="Featured content">
    <h1>Stranger Things Season 4</h1>
    <button aria-label="Play Stranger Things">▶</button>
  </section>
  
  <section aria-label="Action & Adventure">
    <h2>Action & Adventure</h2>
    <div role="list">
      <article role="listitem">
        <img src="show1.jpg" alt="Show 1 poster">
        <h3>Show Title</h3>
      </article>
    </div>
  </section>
</main>
```

#### GitHub (Forms + Accessibility)
GitHub has complex forms for code review:

```html
<form>
  <label for="comment">Comment</label>
  <textarea 
    id="comment" 
    aria-describedby="comment-help"
    required
  ></textarea>
  <span id="comment-help">Markdown is supported</span>
  
  <button type="submit" aria-label="Comment on PR">
    Comment
  </button>
</form>
```

### Code Examples

#### Complete Accessible Form
```html
<form id="newsletter">
  <fieldset>
    <legend>Newsletter Signup</legend>
    <!-- Fieldset groups related fields -->
    
    <div class="form-group">
      <label for="name">Full Name *</label>
      <input 
        id="name" 
        name="name" 
        type="text" 
        required
        aria-required="true"
      >
      <small id="name-error" class="error"></small>
    </div>
    
    <div class="form-group">
      <label for="email">Email Address *</label>
      <input 
        id="email" 
        name="email" 
        type="email" 
        required
        aria-required="true"
        aria-describedby="email-help"
      >
      <small id="email-help">We'll never share your email</small>
      <small id="email-error" class="error"></small>
    </div>
    
    <div class="form-group">
      <fieldset>
        <legend>Email Frequency</legend>
        <label>
          <input type="radio" name="frequency" value="daily" checked>
          Daily digest
        </label>
        <label>
          <input type="radio" name="frequency" value="weekly">
          Weekly digest
        </label>
      </fieldset>
    </div>
    
    <button type="submit">Subscribe</button>
    <button type="reset">Clear</button>
  </fieldset>
</form>

<script>
// Validation
const form = document.getElementById('newsletter');
const emailInput = form.querySelector('#email');
const emailError = form.querySelector('#email-error');

form.addEventListener('submit', (e) => {
  e.preventDefault();
  
  if (!emailInput.validity.valid) {
    if (emailInput.validity.typeMismatch) {
      emailError.textContent = 'Please enter a valid email';
    } else if (emailInput.validity.valueMissing) {
      emailError.textContent = 'Email is required';
    }
    return;
  }
  
  // Form is valid, submit
  console.log('Form is valid!');
});
</script>
```

### Common Gotchas (The 90% Fail Here)

**Gotcha #1: Heading Order**
```html
<!-- ❌ BAD: Skipping levels (h1 → h3) breaks screen readers -->
<h1>Main Title</h1>
<h3>Subsection</h3>

<!-- ✅ GOOD: Sequential headings -->
<h1>Main Title</h1>
<h2>Subsection</h2>
<h3>Sub-subsection</h3>
```

**Gotcha #2: Alt Text for Decorative Images**
```html
<!-- ❌ BAD: Alt text for decoration -->
<img src="decorative-line.png" alt="decorative line">

<!-- ✅ GOOD: Empty alt for decoration -->
<img src="decorative-line.png" alt="">

<!-- ✅ GOOD: Meaningful alt for content images -->
<img src="screenshot.png" alt="User dashboard showing sales chart">
```

**Gotcha #3: Proper Form Association**
```html
<!-- ❌ BAD: Label not linked to input -->
<label>Email</label>
<input type="email">

<!-- ✅ GOOD: Label linked via for/id -->
<label for="email">Email</label>
<input id="email" type="email">
```

**Gotcha #4: Keyboard Navigation**
```html
<!-- ❌ BAD: Click handler on non-button element -->
<div onclick="handleClick()">Click me</div>
<!-- Users can't activate this with keyboard (Tab + Enter) -->

<!-- ✅ GOOD: Proper button element -->
<button onclick="handleClick()">Click me</button>
<!-- Tab to it, press Enter to activate -->
```

**Gotcha #5: Viewport Meta Tag**
```html
<!-- ❌ BAD: Missing or wrong viewport -->
<!-- Causes mobile zoom issues -->
<meta name="viewport" content="width=device-width">

<!-- ✅ GOOD: Proper viewport -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### Advanced Patterns

#### Custom Element with Slots
```html
<script>
class Card extends HTMLElement {
  connectedCallback() {
    this.attachShadow({ mode: 'open' }).innerHTML = `
      <style>
        :host {
          display: block;
          border: 1px solid #ddd;
          border-radius: 8px;
          padding: 16px;
        }
      </style>
      <div class="card">
        <h2><slot name="title"></slot></h2>
        <div><slot></slot></div>
        <footer><slot name="footer"></slot></footer>
      </div>
    `;
  }
}
customElements.define('custom-card', Card);
</script>

<!-- Usage -->
<custom-card>
  <span slot="title">Card Title</span>
  <p>Card content goes here</p>
  <span slot="footer">Card footer</span>
</custom-card>
```

#### Accessible Dropdown (No JavaScript Needed)
```html
<!-- HTML5 native details/summary element -->
<details>
  <summary role="button" tabindex="0">
    Menu ▼
  </summary>
  <ul role="menu">
    <li><a href="/home">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</details>

<style>
details > ul {
  margin: 0;
  padding-left: 0;
}
</style>
```

---

## 🎬 PART 2: Explain Like I'm 5

### The Story Approach

Imagine you're building a house 🏠. 

HTML is like the **skeleton and frame** of the house—the studs, walls, doors, and windows. It decides where the kitchen is, where the bedrooms are, and what rooms connect to what.

CSS is the **decoration**—the paint color, the wallpaper, the nice furniture. It makes the house look beautiful.

JavaScript is the **smart stuff**—the lights that turn on when you enter a room, the door that opens automatically, the TV that responds to the remote. It makes things DO things.

Without the skeleton (HTML), you can't have a house. The skeleton is the foundation of everything.

### How It Works

**Step 1: Create the Structure**
HTML says: "This is the front door" (button), "This is the kitchen" (form), "This is a list of items" (list). It organizes everything into meaningful pieces.

**Step 2: Make it Pretty**
CSS says: "Make the door red," "Make the kitchen have white walls," "Make the list items bigger text."

**Step 3: Make it Work**
JavaScript says: "When someone clicks the door, open it," "When they submit the form, save the data," "When they scroll, load more items."

### Real Example

Think about logging into YouTube 🎥:

1. **HTML provides the structure:**
   - Text box that says "Email" (label)
   - Input field where you type (input)
   - "Sign In" clickable button (button)
   - "Forgot password?" link (link)

2. **CSS makes it look nice:**
   - Centered on the page
   - White input boxes with gray borders
   - Blue button that stands out
   - Nice fonts and spacing

3. **JavaScript makes it work:**
   - When you type an email, it checks if it's valid
   - When you click "Sign In," it sends your email to YouTube's servers
   - If the email is wrong, it shows an error in red text

HTML is Step 1—without it, there's nothing to make pretty or make work.

### Why This Matters

**For People Who Use Websites:**
- HTML that's organized right = websites work better on phones
- HTML with good labels = screen readers (for blind people) can read it out loud
- HTML with proper structure = websites load faster

**For Developers:**
- Good HTML = easier to add styling later
- Good HTML = easier to add features later
- Good HTML = other developers understand it

**For Companies:**
- Google understands your website better = more people find you
- Websites work for everyone (including disabled people) = more users = more money
- Websites that work well = customers are happy

---

## 🎯 PART 3: INTERVIEW QUESTIONS

### Level 1: EASY (Q1-Q3)

**Q1: "What is semantic HTML and why does it matter?"**

**What interviewer is testing:**
- Understanding of HTML's purpose beyond visual styling
- Awareness of accessibility and SEO
- Knowledge of common semantic elements
- Understanding of web standards

**Perfect Answer:**

"Semantic HTML means using HTML elements that have **meaning**—tags that describe what the content IS, not just how it looks. Instead of using `<div>` for everything, you use `<header>`, `<nav>`, `<main>`, `<article>`, `<footer>`, etc.

Why it matters:

1. **Accessibility**: Screen reader users depend on semantic structure. A `<nav>` is announced as 'navigation region,' helping blind users navigate. A `<div>` just gets 'clickable.'

2. **SEO**: Google crawls your HTML to understand structure. Semantic elements help it understand topics, hierarchy, and content types better.

3. **Maintainability**: Other developers understand the code's intent immediately. `<header>` tells you what that section is—no guessing.

4. **Performance**: Browsers can render and optimize semantic HTML more efficiently.

At Google, semantic HTML is a requirement. Every element must have meaning. We use `<button>` for buttons, `<nav>` for navigation, `<article>` for content—never divs with onclick handlers."

**Common mistakes:**
❌ Saying semantic HTML only matters for Google/SEO
❌ Not mentioning accessibility
❌ Thinking semantic HTML is just about HTML5 updates
✅ Understanding it's foundational to good web development
✅ Knowing real consequences (lawsuits, lost users)

**Follow-up questions:**
- "Can you name 5 semantic elements?"
- "Why is `<button>` better than `<div onclick>`?"
- "How would a screen reader user navigate your site?"

**Score:** 9/10 because the answer shows understanding of why it matters, not just what it is.

---

**Q2: "How do you ensure a form is accessible to keyboard-only users?"**

**What interviewer is testing:**
- Practical accessibility knowledge
- Understanding of keyboard navigation
- Knowledge of form best practices
- Attention to inclusive design

**Perfect Answer:**

"There are several critical things:

1. **Proper Label Association**: Every input must have a `<label>` connected with `for` and `id` attributes. This lets screen readers and keyboard users know what each field is.

```html
<label for="email">Email</label>
<input id="email" type="email" required>
```

2. **Semantic HTML Elements**: Use `<button>`, `<input>`, `<select>` instead of custom divs. These are keyboard-accessible by default.

3. **Tab Order**: Ensure tab order follows visual order. Use `tabindex` sparingly (usually you don't need it). Never trap focus.

4. **Form Validation**: Show errors clearly with `aria-describedby` so users know what's wrong.

5. **Submit Behavior**: Never submit on blur or change. Always use a submit button.

```html
<form>
  <label for="name">Name</label>
  <input id="name" type="text" required>
  
  <label for="email">Email</label>
  <input id="email" type="email" required>
  
  <button type="submit">Submit</button>
  <!-- User can Tab between fields, submit with Enter -->
</form>
```

6. **Error Messages**: Link errors with `aria-describedby`:

```html
<input id="email" type="email" aria-describedby="email-error">
<span id="email-error" role="alert">Invalid email format</span>
```

The key principle: If it works with keyboard alone (Tab, Enter, Arrow keys), it's accessible."

**Common mistakes:**
❌ Using placeholder as label (doesn't work for screen readers)
❌ Using JavaScript-only form submission (no fallback)
❌ Custom components that trap keyboard focus
✅ Testing with keyboard only (Tab, Enter, Arrows)

**Follow-up questions:**
- "What's the difference between `<label>` and placeholder?"
- "How would you test keyboard accessibility?"
- "What is aria-describedby and when do you use it?"

**Score:** 9/10 because the answer is practical, code-based, and shows real understanding.

---

**Q3: "Explain what DOCTYPE does and why it's important."**

**What interviewer is testing:**
- Understanding of HTML fundamentals
- Knowledge of browser rendering modes
- Awareness of standards and compatibility

**Perfect Answer:**

"`<!DOCTYPE html>` tells the browser this document is HTML5 and should be rendered in standards mode (not quirks mode).

**Why it matters:**

1. **Standards Mode**: Browser renders according to web standards
2. **Quirks Mode**: Browser renders like old IE (box model differences, bugs, inconsistencies)

Without DOCTYPE, older browsers would enter Quirks Mode, which meant:
- Box model calculated differently (width property behaved differently)
- CSS floats rendered wrong
- Margins collapsed unexpectedly

```html
<!-- ✅ Modern HTML5 (standards mode) -->
<!DOCTYPE html>
<html>
  <body>
    <!-- Rendered correctly -->
  </body>
</html>

<!-- ❌ No DOCTYPE (triggers quirks mode in older browsers) -->
<html>
  <body>
    <!-- Might render incorrectly -->
  </body>
</html>
```

Today, DOCTYPE is less critical because modern browsers are smart, but it's still **required** by the HTML specification and best practice. You must include it."

**Common mistakes:**
❌ Thinking DOCTYPE doesn't matter anymore
❌ Using old XHTML DOCTYPE (`<!DOCTYPE html PUBLIC ...>`)
❌ Not including DOCTYPE at all
✅ Always including `<!DOCTYPE html>` on every HTML file

**Follow-up questions:**
- "What's the difference between standards mode and quirks mode?"
- "What other meta tags go in the head?"
- "Why should charset be the first meta tag?"

**Score:** 8/10 because good explanation, though could mention backwards compatibility more.

---

### Level 2: MEDIUM (Q4-Q6)

**Q4: "Build an accessible navigation bar using semantic HTML. What elements would you use and why?"**

**What interviewer is testing:**
- Practical semantic HTML knowledge
- Accessibility implementation
- Understanding of navigation best practices
- Code quality and real-world applicability

**Perfect Answer:**

"I'd use `<nav>`, `<ul>`, `<li>`, and `<a>` elements for semantic meaning. Here's why and how:

```html
<nav aria-label="Main navigation">
  <!-- aria-label distinguishes from other nav elements -->
  <ul>
    <!-- <ul> tells screen readers it's a list -->
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>

<!-- Why this works:
1. <nav> identifies this as navigation
2. <ul> indicates it's a list
3. Screen reader announces: 'Navigation list, 4 items'
4. Keyboard users can Tab through links
5. Easy to style with CSS (li:hover, etc.)
-->
```

**For dropdown menus:**

```html
<nav aria-label="Main navigation">
  <ul>
    <li>
      <a href="/products" aria-haspopup="true" aria-expanded="false">
        Products ▼
      </a>
      <ul>
        <li><a href="/products/electronics">Electronics</a></li>
        <li><a href="/products/clothing">Clothing</a></li>
        <li><a href="/products/home">Home & Garden</a></li>
      </ul>
    </li>
  </ul>
</nav>

<!-- aria-haspopup tells screen readers there's a submenu
     aria-expanded toggles with JavaScript when opened/closed
-->
```

**JavaScript for accessibility:**

```javascript
const toggle = document.querySelector('[aria-haspopup]');
const menu = toggle.nextElementSibling;

toggle.addEventListener('click', () => {
  const isExpanded = toggle.getAttribute('aria-expanded') === 'true';
  toggle.setAttribute('aria-expanded', !isExpanded);
  menu.hidden = isExpanded; // Toggle visibility
});

// Keyboard support
toggle.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowDown') {
    toggle.click(); // Open menu
    menu.querySelector('a').focus(); // Focus first item
  }
});
```

**Why these choices:
1. `<nav>` = semantic meaning (this is navigation)
2. `<ul>/<li>` = tells screen readers it's a list with items
3. `<a>` = keyboard accessible by default
4. `aria-label` = distinguishes multiple nav elements
5. `aria-haspopup` = indicates there's a submenu
6. Keyboard support = works without mouse**"

**Common mistakes:**
❌ Using `<div>` instead of `<nav>`
❌ Using `onclick` handlers instead of semantic elements
❌ Not including aria labels for screen readers
❌ Forgetting keyboard support (arrow keys)
✅ Semantic HTML first, then enhance with JS
✅ Testing with keyboard AND screen reader

**Follow-up questions:**
- "How would you test this with a screen reader?"
- "What if you had 100 navigation items? Would this scale?"
- "How would you handle mobile navigation?"

**Score:** 9/10 because practical, accessible, and shows understanding of semantics + ARIA.

---

**Q5: "How do you handle form validation? What's the difference between HTML5 validation and custom validation?"**

**What interviewer is testing:**
- Understanding of HTML5 features
- Practical validation implementation
- Knowledge of when to use native vs custom
- Understanding of user experience

**Perfect Answer:**

"HTML5 provides **built-in validation** for free, but sometimes you need **custom validation** for complex rules.

**HTML5 Validation (Use This First):**

```html
<form>
  <!-- Email validation -->
  <input type="email" required>
  
  <!-- Number range -->
  <input type="number" min="18" max="120" required>
  
  <!-- Password minimum length -->
  <input type="password" minlength="8" required>
  
  <!-- Pattern matching (regex) -->
  <input 
    type="text" 
    pattern="[A-Z]{2}\d{3}"
    title="Format: AB123"
  >
  
  <!-- URL validation -->
  <input type="url" required>
  
  <!-- Browser shows errors automatically -->
  <button type="submit">Submit</button>
</form>

<!-- Advantages:
1. Works without JavaScript
2. Browser shows native error messages
3. Mobile keyboards adapt (email keyboard for email, etc.)
4. Built-in accessibility
-->
```

**Custom Validation (For Complex Rules):**

```html
<form id="form">
  <label for="password">Password</label>
  <input 
    id="password" 
    type="password"
    aria-describedby="password-requirements"
  >
  <div id="password-requirements" class="requirements">
    <p>Must contain:</p>
    <ul>
      <li id="req-length">At least 8 characters</li>
      <li id="req-uppercase">One uppercase letter (A-Z)</li>
      <li id="req-number">One number (0-9)</li>
      <li id="req-special">One special character (!@#$)</li>
    </ul>
  </div>
  
  <button type="submit">Create Account</button>
</form>

<script>
const password = document.getElementById('password');

password.addEventListener('input', (e) => {
  const value = e.target.value;
  
  // Check each requirement
  const hasLength = value.length >= 8;
  const hasUppercase = /[A-Z]/.test(value);
  const hasNumber = /\d/.test(value);
  const hasSpecial = /[!@#$%^&*]/.test(value);
  
  // Update visual feedback
  document.getElementById('req-length').classList.toggle('valid', hasLength);
  document.getElementById('req-uppercase').classList.toggle('valid', hasUppercase);
  document.getElementById('req-number').classList.toggle('valid', hasNumber);
  document.getElementById('req-special').classList.toggle('valid', hasSpecial);
  
  // Disable submit if invalid
  const isValid = hasLength && hasUppercase && hasNumber && hasSpecial;
  document.querySelector('button').disabled = !isValid;
});

// Prevent submit if invalid
document.getElementById('form').addEventListener('submit', (e) => {
  // Custom validation logic
  if (!password.value.match(/^(?=.*[A-Z])(?=.*\d)(?=.*[!@#$])(?=.{8,})/)) {
    e.preventDefault();
    password.setCustomValidity('Password does not meet requirements');
    return;
  }
  
  password.setCustomValidity(''); // Clear error
});
</script>
```

**When to use what:**
- **HTML5 Validation**: Email, phone, URL, number range, required fields
- **Custom Validation**: Password strength, matching fields, business logic

**Best Practice: Combine Both:**

```html
<input 
  type="email" 
  required 
  id="email"
>

<script>
const email = document.getElementById('email');

email.addEventListener('blur', async () => {
  // HTML5 says it's a valid email format
  if (!email.validity.valid) return;
  
  // Custom: Check if email exists in database
  const response = await fetch(`/api/email-exists?email=${email.value}`);
  const exists = await response.json();
  
  if (exists) {
    email.setCustomValidity('Email already registered');
  }
});
</script>
```

**Why this approach:
1. Use HTML5 for format validation (catches 80% of errors)
2. Use custom for complex rules
3. Users get fast feedback
4. Works with and without JavaScript
5. Accessible to all users**"

**Common mistakes:**
❌ Relying only on HTML5 (no server-side validation)
❌ Complex custom validation without HTML5 base
❌ Not showing users what's wrong
❌ Preventing form submission without explaining why
✅ Validate on client AND server
✅ Give real-time feedback
✅ Show clear error messages

**Follow-up questions:**
- "Why do you need server-side validation if HTML5 validates?"
- "How would you handle password confirmation?"
- "How would you test validation?"

**Score:** 10/10 because comprehensive, practical, and shows understanding of both approaches.

---

**Q6: "How do you implement proper heading hierarchy? Why does it matter?"**

**What interviewer is testing:**
- Understanding of semantic structure
- Accessibility knowledge
- SEO awareness
- Code organization skills

**Perfect Answer:**

"Heading hierarchy (`<h1>` through `<h6>`) organizes content into a logical outline. It matters for accessibility and SEO.

**What NOT to do (Common Mistakes):**

```html
<!-- ❌ WRONG: Skipping levels -->
<h1>Main Title</h1>
<h3>Subsection</h3>  <!-- Should be h2! -->
<h4>Sub-subsection</h4>  <!-- And h3 -->

<!-- ❌ WRONG: Multiple h1s -->
<h1>Company Logo</h1>
<h1>Company Tagline</h1>
<h1>Main Article Title</h1>

<!-- ❌ WRONG: Using heading for styling only -->
<h1 style="font-size: 12px;">Small title</h1>
```

**Correct Approach:**

```html
<!-- ✅ CORRECT: Sequential, logical hierarchy -->
<h1>Main Article: The Rise of AI</h1>

<section>
  <h2>Introduction</h2>
  <p>AI has transformed...</p>
</section>

<section>
  <h2>Chapter 1: Neural Networks</h2>
  <p>Neural networks are...</p>
  
  <h3>1.1 History of Neural Networks</h3>
  <p>The first neural networks...</p>
  
  <h3>1.2 How They Work Today</h3>
  <p>Modern neural networks use...</p>
</section>

<section>
  <h2>Chapter 2: Deep Learning</h2>
  <p>Deep learning builds on...</p>
  
  <h3>2.1 Introduction</h3>
  <h3>2.2 Applications</h3>
</section>

<section>
  <h2>Conclusion</h2>
  <p>In conclusion...</p>
</section>
```

**How this helps:

1. **Screen Readers**: Blind users can 'skip to next heading' to scan the page structure. If hierarchy is broken, they get lost.

   Example: Screen reader user presses 'H' to jump to next heading:
   - H1: Main Article
   - H2: Introduction
   - H2: Chapter 1 (skipped h3s to get back to main sections)
   - H2: Conclusion

2. **Search Engines**: Google understands content structure from heading hierarchy:
   - H1 = page's main topic
   - H2s = major sections
   - H3s = subsections

   Better structure = better SEO ranking

3. **Usability**: Users can scan page outline quickly

**One H1 Per Page (Best Practice):**

```html
<!-- ✅ CORRECT: One main h1, rest are hierarchical -->
<h1>How to Make Coffee</h1>

<section>
  <h2>What You Need</h2>
</section>

<section>
  <h2>Step-by-Step Instructions</h2>
  <h3>Step 1: Grind Beans</h3>
  <h3>Step 2: Brew</h3>
</section>

<!-- NOT:
<h1>How to Make Coffee</h1>
<h1>Step 1: Grind Beans</h1>
<h1>Step 2: Brew</h1>
This confuses screen readers and Google.
-->
```

**Testing Your Heading Structure:**

```javascript
// Scan for heading issues
const headings = Array.from(document.querySelectorAll('h1, h2, h3, h4, h5, h6'));
const levels = headings.map(h => parseInt(h.tagName[1]));

// Check for skips
for (let i = 1; i < levels.length; i++) {
  if (levels[i] - levels[i-1] > 1) {
    console.warn(`Skipped level: ${levels[i-1]} → ${levels[i]}`);
  }
}

// Check for multiple h1s
const h1s = document.querySelectorAll('h1');
if (h1s.length > 1) {
  console.warn(`Found ${h1s.length} h1s (should be 1)`);
}
```

**Why it matters in production:
- Netflix: Heading hierarchy helps users scan show categories
- GitHub: Heading structure helps developers understand file organization
- Medium: Proper h1-h6 helps article discovery
- Google Docs: Heading structure enables automatic table of contents**"

**Common mistakes:**
❌ Using h1 for styling (when you want small text)
❌ Skipping heading levels
❌ Multiple h1s per page
❌ Using headings that don't reflect content
✅ Semantic h1-h6 structure
✅ One h1 per page

**Follow-up questions:**
- "Can you use h1 if you only want the small text styling?"
- "Should you use h1 for a logo?"
- "What if you have no h2, just multiple h3s?"

**Score:** 9/10 because explains why, shows common mistakes, and practical examples.

---

### Level 3: HARD (Q7-Q10)

**Q7: "Design an accessible data table with sorting and filtering. What HTML structure would you use?"**

**What interviewer is testing:**
- Advanced semantic HTML knowledge
- Complex accessibility implementation
- Real-world problem solving
- ARIA attributes mastery
- JavaScript integration understanding

**Perfect Answer:**

"An accessible data table needs proper HTML structure, ARIA attributes, and JavaScript for interactivity while maintaining accessibility.

```html
<div role="region" aria-label="Searchable employee table" aria-live="polite">
  <!-- aria-live announces table updates (sorting, filtering) -->
  
  <div class="table-controls">
    <label for="search">Search employees:</label>
    <input id="search" type="text" placeholder="Name, email...">
    
    <label for="department">Filter by department:</label>
    <select id="department">
      <option value="">All departments</option>
      <option value="engineering">Engineering</option>
      <option value="product">Product</option>
    </select>
  </div>
  
  <table aria-describedby="table-description">
    <caption id="table-description">
      Employee Directory - Click column headers to sort
    </caption>
    
    <thead>
      <tr>
        <th scope="col">
          <button 
            aria-sort="ascending" 
            data-column="name"
            class="sortable-header"
          >
            Name
            <span aria-label="sorted ascending" class="sort-indicator">↑</span>
          </button>
        </th>
        
        <th scope="col">
          <button 
            aria-sort="none" 
            data-column="email"
            class="sortable-header"
          >
            Email
            <span aria-label="not sorted" class="sort-indicator"></span>
          </button>
        </th>
        
        <th scope="col">
          <button 
            aria-sort="none" 
            data-column="department"
            class="sortable-header"
          >
            Department
            <span aria-label="not sorted" class="sort-indicator"></span>
          </button>
        </th>
      </tr>
    </thead>
    
    <tbody id="table-body">
      <tr>
        <td>Alice Johnson</td>
        <td>alice@company.com</td>
        <td>Engineering</td>
      </tr>
      <tr>
        <td>Bob Smith</td>
        <td>bob@company.com</td>
        <td>Product</td>
      </tr>
      <!-- More rows -->
    </tbody>
  </table>
  
  <div id="table-status" class="sr-only" role="status" aria-live="polite">
    <!-- Screen readers announce: '12 employees, Engineering department' -->
  </div>
</div>

<script>
class DataTable {
  constructor(tableElement) {
    this.table = tableElement;
    this.data = this.parseTable();
    this.sortColumn = 'name';
    this.sortDirection = 'ascending';
    
    this.setupSorting();
    this.setupFiltering();
  }
  
  parseTable() {
    // Extract data from table
    return Array.from(this.table.querySelectorAll('tbody tr')).map(row => ({
      name: row.cells[0].textContent,
      email: row.cells[1].textContent,
      department: row.cells[2].textContent
    }));
  }
  
  setupSorting() {
    const headers = this.table.querySelectorAll('.sortable-header');
    
    headers.forEach(header => {
      header.addEventListener('click', () => {
        const column = header.dataset.column;
        
        // Toggle sort direction
        if (this.sortColumn === column) {
          this.sortDirection = this.sortDirection === 'ascending' ? 'descending' : 'ascending';
        } else {
          this.sortColumn = column;
          this.sortDirection = 'ascending';
        }
        
        // Update ARIA sort attributes
        headers.forEach(h => h.setAttribute('aria-sort', 'none'));
        header.setAttribute('aria-sort', this.sortDirection);
        
        // Sort and re-render
        this.sortData();
        this.renderTable();
        
        // Announce to screen readers
        this.announceStatus(`Sorted by ${column}, ${this.sortDirection}`);
      });
    });
  }
  
  sortData() {
    this.data.sort((a, b) => {
      const aVal = a[this.sortColumn];
      const bVal = b[this.sortColumn];
      
      const comparison = aVal.localeCompare(bVal);
      return this.sortDirection === 'ascending' ? comparison : -comparison;
    });
  }
  
  setupFiltering() {
    const searchInput = document.getElementById('search');
    const departmentSelect = document.getElementById('department');
    
    const filter = () => {
      const searchTerm = searchInput.value.toLowerCase();
      const department = departmentSelect.value;
      
      const filtered = this.data.filter(row => {
        const matchesSearch = 
          row.name.toLowerCase().includes(searchTerm) ||
          row.email.toLowerCase().includes(searchTerm);
        
        const matchesDept = !department || row.department === department;
        
        return matchesSearch && matchesDept;
      });
      
      this.renderTable(filtered);
      this.announceStatus(`Showing ${filtered.length} of ${this.data.length} employees`);
    };
    
    searchInput.addEventListener('input', filter);
    departmentSelect.addEventListener('change', filter);
  }
  
  renderTable(data = this.data) {
    const tbody = document.getElementById('table-body');
    tbody.innerHTML = '';
    
    data.forEach(row => {
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td>${row.name}</td>
        <td>${row.email}</td>
        <td>${row.department}</td>
      `;
      tbody.appendChild(tr);
    });
  }
  
  announceStatus(message) {
    const status = document.getElementById('table-status');
    status.textContent = message;
  }
}

// Initialize
const table = document.querySelector('table');
new DataTable(table);
</script>
```

**Key Accessibility Features:

1. **<caption>**: Describes the table to screen readers
2. **scope='col'**: Tells screen readers which header applies to which column
3. **aria-sort**: Shows current sort state (ascending, descending, none)
4. **aria-live**: Announces updates (sorting, filtering) to screen readers
5. **role='region'**: Groups related content
6. **aria-describedby**: Links table to description
7. **sr-only**: Screen reader only class for status updates

**Why this matters:
- Blind users can understand and use the table
- Screen readers announce sort changes
- Keyboard users can navigate and sort
- Data updates are announced (aria-live)
- Clear visual + audio feedback**"

**Common mistakes:**
❌ Tables for layout instead of data
❌ Missing `<thead>` and `<tbody>`
❌ No `scope` attribute
❌ Sorting without ARIA updates
❌ Filtering without announcing results
✅ Semantic table structure
✅ ARIA attributes for dynamic changes
✅ Keyboard + screen reader testing

**Follow-up questions:**
- "How would you handle pagination?"
- "What if you had hundreds of thousands of rows?"
- "How would you make column widths responsive?"

**Score:** 10/10 because comprehensive, accessible, and production-ready.

---

**Q8: "Explain Web Components and when you would use them instead of a framework like React."**

**What interviewer is testing:**
- Advanced HTML5 knowledge
- Understanding of Web Components standards
- Decision-making about tool selection
- Knowledge of custom elements, shadow DOM, slots

**Perfect Answer:**

"Web Components are **native browser APIs** for creating reusable custom HTML elements. They consist of three technologies:

1. **Custom Elements API**: Define your own HTML tags
2. **Shadow DOM**: Encapsulate styles and structure
3. **HTML Templates**: Define reusable content

**Example - Building a Reusable Card Component:**

```html
<!-- Define the component -->
<script>
class CardComponent extends HTMLElement {
  constructor() {
    super();
    // Shadow DOM isolates styles
    this.attachShadow({ mode: 'open' });
  }
  
  connectedCallback() {
    // Render when added to DOM
    this.render();
  }
  
  render() {
    this.shadowRoot.innerHTML = `
      <style>
        :host {
          display: block;
          border: 1px solid #ddd;
          border-radius: 8px;
          padding: 16px;
          background: white;
        }
        
        h2 {
          margin-top: 0;
        }
      </style>
      
      <div class="card">
        <h2><slot name='title'></slot></h2>
        <p><slot></slot></p>
        <footer><slot name='footer'></slot></footer>
      </div>
    `;
  }
  
  // Custom attributes
  static get observedAttributes() {
    return ['title', 'color'];
  }
  
  attributeChangedCallback(name, oldVal, newVal) {
    // React when attributes change
    this.render();
  }
}

// Register the custom element
customElements.define('custom-card', CardComponent);
</script>

<!-- Use the component anywhere -->
<custom-card title='Hello'>
  This is the card content
  <span slot='footer'>Footer text</span>
</custom-card>
```

**When to Use Web Components:**

1. **Micro Frontends**: Different teams build separate components
   - Team A builds login component
   - Team B builds dashboard component
   - They work independently, share via Web Components

2. **Reusable UI Library**: Build once, use in React, Vue, Angular, or vanilla JS
   - Google: Material Web Components
   - Salesforce: LWC (Lightning Web Components)
   - Adobe: Spectrum Web Components

3. **Legacy System Integration**: Add new features to old jQuery/PHP apps
   - Install a Web Component
   - Use it immediately, no framework needed

4. **Framework-Agnostic Code**: Code that works everywhere
   - Works in React (with React.createElement)
   - Works in Vue (native support)
   - Works in vanilla JS
   - Works in HTML with zero JS

**Example - Using Web Component in Different Frameworks:**

```javascript
// React
function App() {
  return (
    <custom-card title="React">
      This works in React too!
    </custom-card>
  );
}

// Vue
<template>
  <custom-card title="Vue">
    This works in Vue too!
  </custom-card>
</template>

// Vanilla JS
document.body.innerHTML = `
  <custom-card title="Vanilla">
    This works everywhere!
  </custom-card>
`;
```

**Web Components vs React:**

| Aspect | Web Components | React |
|--------|----------------|-------|
| **Learning Curve** | Low (native APIs) | Medium (library-specific) |
| **Bundle Size** | ~0KB (native) | ~30-40KB (React) |
| **Ecosystem** | Small | Massive |
| **Reusability** | Framework-agnostic | React-only |
| **State Management** | Component-level | Flexible (Redux, etc.) |
| **Best For** | Simple reusable components | Complex applications |

**Real-World Example - Google:**

Google uses Web Components for parts of Gmail:

```html
<!-- Google's Web Components in Gmail -->
<gm-action-bar>
  <!-- Shadow DOM encapsulates Gmail's styling -->
  <!-- Doesn't conflict with user CSS in emails -->
</gm-action-bar>

<gm-email>
  <!-- Displays email in isolated Shadow DOM -->
</gm-email>
```

Why? Email content can contain any CSS. Shadow DOM prevents it from breaking Gmail's UI.

**Limitations of Web Components:**

- ❌ No built-in state management
- ❌ No built-in routing
- ❌ Steeper learning curve for complex logic
- ❌ Limited debugging tools
- ❌ Smaller ecosystem than React

**When NOT to Use Web Components:**
- Complex single-page applications (use React)
- Heavy state management (use Redux + React)
- Large team with React expertise
- Need mature ecosystem

**Best Practice: Combine Both:**

```html
<!-- Use Web Components for reusable UI -->
<custom-button></custom-button>
<custom-dialog></custom-dialog>

<!-- Use React for app logic -->
<react-app>
  <component-using-web-components />
</react-app>
```

At Google, we use Web Components for UI components (buttons, cards, dialogs) that need to work across different products. We use React/Angular for product-specific logic."

**Common mistakes:**
❌ Using Web Components for everything (overkill)
❌ Not using Shadow DOM (styles conflict)
❌ Not exposing API via properties/methods
❌ Tight coupling to Web Components (hard to extract)
✅ Use for reusable, framework-agnostic components
✅ Properly encapsulate with Shadow DOM
✅ Expose clean API

**Follow-up questions:**
- "How would you handle state management in Web Components?"
- "How would you test Web Components?"
- "Can you use Web Components inside a React component?"

**Score:** 10/10 because comprehensive, shows decision-making, and real-world context.

---

**Q9: "How would you implement a completely accessible wizard/multi-step form? What HTML, ARIA, and JavaScript would you use?"**

**What interviewer is testing:**
- Advanced accessibility implementation
- Complex HTML structure
- ARIA mastery
- Real-world problem solving
- User experience thinking

**Perfect Answer:**

"A wizard form is a multi-step form where users progress through steps. It needs clear structure, progress indication, and full accessibility.

```html
<div role="region" aria-label="Account setup wizard">
  <div class="wizard-progress" aria-label="Progress">
    <h2 id="wizard-title">Account Setup</h2>
    
    <div class="progress-bar">
      <div class="progress-fill" style="width: 33%"></div>
    </div>
    
    <ol class="step-indicator" aria-label="Steps">
      <li aria-current="step">
        <span aria-label="Step 1 of 3: Email, current step">1</span>
        Email
      </li>
      <li>
        <span aria-label="Step 2 of 3: Password">2</span>
        Password
      </li>
      <li>
        <span aria-label="Step 3 of 3: Profile">3</span>
        Profile
      </li>
    </ol>
  </div>
  
  <form id="wizard-form" aria-labelledby="wizard-title">
    <!-- Step 1: Email -->
    <fieldset id="step-1" class="wizard-step active">
      <legend>Email</legend>
      
      <label for="email">Email Address *</label>
      <input 
        id="email" 
        name="email" 
        type="email" 
        required
        aria-required="true"
      >
      
      <button 
        type="button" 
        class="btn-next"
        data-step="2"
        aria-label="Next: Password"
      >
        Continue
      </button>
    </fieldset>
    
    <!-- Step 2: Password -->
    <fieldset id="step-2" class="wizard-step" hidden aria-hidden="true">
      <legend>Create Password</legend>
      
      <label for="password">Password *</label>
      <input 
        id="password" 
        name="password" 
        type="password" 
        minlength="8" 
        required
        aria-required="true"
      >
      
      <label for="password-confirm">Confirm Password *</label>
      <input 
        id="password-confirm" 
        name="password-confirm" 
        type="password" 
        required
        aria-required="true"
      >
      
      <div class="button-group">
        <button 
          type="button" 
          class="btn-prev"
          data-step="1"
          aria-label="Back: Email"
        >
          Back
        </button>
        
        <button 
          type="button" 
          class="btn-next"
          data-step="3"
          aria-label="Next: Profile"
        >
          Continue
        </button>
      </div>
    </fieldset>
    
    <!-- Step 3: Profile -->
    <fieldset id="step-3" class="wizard-step" hidden aria-hidden="true">
      <legend>Profile Information</legend>
      
      <label for="name">Full Name *</label>
      <input 
        id="name" 
        name="name" 
        type="text" 
        required
        aria-required="true"
      >
      
      <label for="bio">Bio</label>
      <textarea id="bio" name="bio"></textarea>
      
      <div class="button-group">
        <button 
          type="button" 
          class="btn-prev"
          data-step="2"
          aria-label="Back: Password"
        >
          Back
        </button>
        
        <button 
          type="submit" 
          aria-label="Complete account setup"
        >
          Create Account
        </button>
      </div>
    </fieldset>
    
    <!-- Status updates for screen readers -->
    <div id="wizard-status" role="status" aria-live="polite" class="sr-only">
      <!-- Screen readers announce: 'Step 1 of 3: Email' -->
    </div>
  </form>
</div>

<style>
.wizard-step {
  display: none;
}

.wizard-step.active {
  display: block;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
}
</style>

<script>
class WizardForm {
  constructor(formElement) {
    this.form = formElement;
    this.currentStep = 1;
    this.totalSteps = 3;
    
    this.setupEventListeners();
    this.updateUI();
  }
  
  setupEventListeners() {
    // Next button
    this.form.querySelectorAll('.btn-next').forEach(btn => {
      btn.addEventListener('click', () => {
        if (this.validateStep(this.currentStep)) {
          this.currentStep = parseInt(btn.dataset.step);
          this.updateUI();
        }
      });
    });
    
    // Previous button
    this.form.querySelectorAll('.btn-prev').forEach(btn => {
      btn.addEventListener('click', () => {
        this.currentStep = parseInt(btn.dataset.step);
        this.updateUI();
      });
    });
    
    // Form submission
    this.form.addEventListener('submit', (e) => {
      e.preventDefault();
      if (this.validateStep(this.currentStep)) {
        console.log('Form submitted!');
        // Send data to server
      }
    });
    
    // Keyboard support (Enter to continue)
    this.form.addEventListener('keypress', (e) => {
      if (e.key === 'Enter' && e.target.tagName === 'INPUT') {
        const nextBtn = this.getCurrentStep().querySelector('.btn-next');
        if (nextBtn) nextBtn.click();
      }
    });
  }
  
  getCurrentStep() {
    return document.getElementById(`step-${this.currentStep}`);
  }
  
  validateStep(stepNum) {
    const step = document.getElementById(`step-${stepNum}`);
    
    // Get required inputs
    const requiredInputs = step.querySelectorAll('[required]');
    let isValid = true;
    
    requiredInputs.forEach(input => {
      if (!input.validity.valid) {
        isValid = false;
        input.setAttribute('aria-invalid', 'true');
      } else {
        input.removeAttribute('aria-invalid');
      }
    });
    
    // Step-specific validation
    if (stepNum === 2) {
      const password = document.getElementById('password');
      const confirm = document.getElementById('password-confirm');
      
      if (password.value !== confirm.value) {
        confirm.setAttribute('aria-invalid', 'true');
        return false;
      }
    }
    
    return isValid;
  }
  
  updateUI() {
    // Hide all steps
    this.form.querySelectorAll('.wizard-step').forEach((step, i) => {
      step.classList.remove('active');
      step.hidden = true;
      step.removeAttribute('aria-hidden');
    });
    
    // Show current step
    this.getCurrentStep().classList.add('active');
    this.getCurrentStep().hidden = false;
    this.getCurrentStep().removeAttribute('aria-hidden');
    
    // Focus first input of current step
    const firstInput = this.getCurrentStep().querySelector('input');
    if (firstInput) firstInput.focus();
    
    // Update progress
    this.updateProgress();
    
    // Announce to screen readers
    this.announceStep();
  }
  
  updateProgress() {
    const progressFill = document.querySelector('.progress-fill');
    const percentage = (this.currentStep / this.totalSteps) * 100;
    progressFill.style.width = percentage + '%';
    
    // Update step indicator
    document.querySelectorAll('.step-indicator li').forEach((li, i) => {
      if (i + 1 === this.currentStep) {
        li.setAttribute('aria-current', 'step');
      } else {
        li.removeAttribute('aria-current');
      }
    });
  }
  
  announceStep() {
    const stepName = this.getCurrentStep().querySelector('legend').textContent;
    const message = `Step ${this.currentStep} of ${this.totalSteps}: ${stepName}`;
    
    const status = document.getElementById('wizard-status');
    status.textContent = message;
  }
}

// Initialize
document.addEventListener('DOMContentLoaded', () => {
  new WizardForm(document.getElementById('wizard-form'));
});
</script>
```

**Key Accessibility Features:

1. **fieldset/legend**: Groups related fields with a label
2. **aria-current='step'**: Shows current step in list
3. **aria-hidden**: Hides inactive steps from screen readers
4. **aria-live='polite'**: Announces step changes
5. **aria-invalid**: Shows validation errors
6. **aria-label**: Descriptive button labels
7. **role='status'**: Screen reader status announcements
8. **Keyboard support**: Enter key, arrow keys for navigation

**Why this matters:
- Screen reader users hear 'Step 1 of 3: Email'
- Keyboard users can navigate with Tab + Enter
- Mobile users see clear progress
- Validation errors are announced
- Form is completely usable without mouse**"

**Common mistakes:**
❌ Showing/hiding steps with display:none (not accessible)
❌ No focus management between steps
❌ No validation feedback
❌ Unclear step progression
❌ Not announcing step changes
✅ Proper ARIA attributes
✅ Focus management
✅ Clear progress indication
✅ Screen reader announcements

**Follow-up questions:**
- "How would you save progress if user closes the browser?"
- "What if steps depend on each other?"
- "How would you track analytics for each step?"

**Score:** 10/10 because production-ready, fully accessible, and shows deep understanding.

---

**Q10: "Design an accessible image gallery with lightbox. How would you handle keyboard navigation and screen readers?"**

**What interviewer is testing:**
- Advanced accessibility implementation
- Complex interactive component
- Real-world problem solving
- Focus management
- ARIA attributes

**Perfect Answer:**

"An image gallery with lightbox needs keyboard navigation, screen reader support, and focus trap management.

```html
<section class="gallery" aria-label="Product images">
  <h2>Gallery</h2>
  
  <!-- Image Grid -->
  <ul class="gallery-grid" role="grid">
    <li role="gridcell">
      <button 
        class="gallery-item"
        data-image-id="1"
        aria-label="Product image 1 of 6. Click to open lightbox"
      >
        <img src="image1-thumb.jpg" alt="Product image 1">
      </button>
    </li>
    
    <li role="gridcell">
      <button 
        class="gallery-item"
        data-image-id="2"
        aria-label="Product image 2 of 6. Click to open lightbox"
      >
        <img src="image2-thumb.jpg" alt="Product image 2">
      </button>
    </li>
    
    <!-- More items -->
  </ul>
</section>

<!-- Lightbox Modal -->
<div 
  id="lightbox" 
  class="lightbox" 
  role="dialog" 
  aria-modal="true" 
  aria-labelledby="lightbox-title"
  aria-hidden="true"
  hidden
>
  <div class="lightbox-content">
    <button 
      class="lightbox-close" 
      aria-label="Close gallery (Press Escape)"
      tabindex="0"
    >
      ✕
    </button>
    
    <h2 id="lightbox-title" class="sr-only">Image Gallery Lightbox</h2>
    
    <!-- Previous/Next Navigation -->
    <button 
      class="lightbox-prev" 
      aria-label="Previous image"
      tabindex="0"
    >
      ❮
    </button>
    
    <!-- Main Image -->
    <figure>
      <img 
        id="lightbox-image" 
        src="" 
        alt=""
        role="img"
        aria-label="Full size image"
      >
      <figcaption id="image-counter">1 / 6</figcaption>
    </figure>
    
    <button 
      class="lightbox-next" 
      aria-label="Next image"
      tabindex="0"
    >
      ❯
    </button>
    
    <!-- Thumbnail Strip -->
    <div class="lightbox-thumbnails" role="region" aria-label="Thumbnail navigation">
      <ul role="list">
        <li role="presentation">
          <button 
            class="thumbnail" 
            data-image-id="1"
            aria-label="Go to image 1"
            aria-current="page"
          >
            <img src="image1-thumb.jpg" alt="">
          </button>
        </li>
        
        <!-- More thumbnails -->
      </ul>
    </div>
    
    <!-- Screen reader only instructions -->
    <div class="sr-only" id="lightbox-instructions" role="region">
      Use arrow keys to navigate between images, or click thumbnails. 
      Press Escape to close the gallery.
    </div>
  </div>
  
  <!-- Backdrop -->
  <div class="lightbox-backdrop"></div>
</div>

<style>
.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.lightbox[aria-hidden='true'] {
  display: none;
}

.lightbox-backdrop {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.9);
}

.lightbox-content {
  position: relative;
  z-index: 1001;
  max-width: 90vw;
  max-height: 90vh;
  display: flex;
  align-items: center;
  gap: 20px;
}

/* Focus visible for keyboard users */
.lightbox button:focus-visible {
  outline: 3px solid yellow;
  outline-offset: 2px;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
}
</style>

<script>
class ImageGallery {
  constructor() {
    this.images = [
      { id: 1, src: 'image1.jpg', alt: 'Product image 1' },
      { id: 2, src: 'image2.jpg', alt: 'Product image 2' },
      { id: 3, src: 'image3.jpg', alt: 'Product image 3' },
      // More images
    ];
    
    this.currentImageIndex = 0;
    this.lightbox = document.getElementById('lightbox');
    this.previousFocus = null; // For focus restoration
    
    this.setupEventListeners();
  }
  
  setupEventListeners() {
    // Open lightbox from gallery items
    document.querySelectorAll('.gallery-item').forEach((btn, index) => {
      btn.addEventListener('click', () => {
        this.currentImageIndex = index;
        this.openLightbox();
      });
    });
    
    // Close button
    document.querySelector('.lightbox-close').addEventListener('click', () => {
      this.closeLightbox();
    });
    
    // Previous/Next buttons
    document.querySelector('.lightbox-prev').addEventListener('click', () => {
      this.previousImage();
    });
    
    document.querySelector('.lightbox-next').addEventListener('click', () => {
      this.nextImage();
    });
    
    // Thumbnail navigation
    document.querySelectorAll('.thumbnail').forEach((thumb, index) => {
      thumb.addEventListener('click', () => {
        this.currentImageIndex = index;
        this.updateLightbox();
      });
    });
    
    // Keyboard support
    document.addEventListener('keydown', (e) => {
      if (this.lightbox.hidden) return;
      
      switch (e.key) {
        case 'ArrowLeft':
          e.preventDefault();
          this.previousImage();
          break;
        case 'ArrowRight':
          e.preventDefault();
          this.nextImage();
          break;
        case 'Escape':
          e.preventDefault();
          this.closeLightbox();
          break;
        case 'Home':
          e.preventDefault();
          this.currentImageIndex = 0;
          this.updateLightbox();
          break;
        case 'End':
          e.preventDefault();
          this.currentImageIndex = this.images.length - 1;
          this.updateLightbox();
          break;
      }
    });
    
    // Close on backdrop click
    document.querySelector('.lightbox-backdrop').addEventListener('click', () => {
      this.closeLightbox();
    });
  }
  
  openLightbox() {
    this.previousFocus = document.activeElement; // Save focus
    
    this.lightbox.removeAttribute('hidden');
    this.lightbox.setAttribute('aria-hidden', 'false');
    
    // Focus the close button
    document.querySelector('.lightbox-close').focus();
    
    // Trap focus inside lightbox
    this.setupFocusTrap();
    
    this.updateLightbox();
  }
  
  closeLightbox() {
    this.lightbox.setAttribute('hidden', '');
    this.lightbox.setAttribute('aria-hidden', 'true');
    
    // Restore focus to the button that opened the lightbox
    if (this.previousFocus) {
      this.previousFocus.focus();
    }
  }
  
  previousImage() {
    this.currentImageIndex = (this.currentImageIndex - 1 + this.images.length) % this.images.length;
    this.updateLightbox();
  }
  
  nextImage() {
    this.currentImageIndex = (this.currentImageIndex + 1) % this.images.length;
    this.updateLightbox();
  }
  
  updateLightbox() {
    const image = this.images[this.currentImageIndex];
    const img = document.getElementById('lightbox-image');
    const counter = document.querySelector('figcaption');
    
    img.src = image.src;
    img.alt = image.alt;
    counter.textContent = `${this.currentImageIndex + 1} / ${this.images.length}`;
    
    // Update which thumbnail is current
    document.querySelectorAll('.thumbnail').forEach((thumb, i) => {
      if (i === this.currentImageIndex) {
        thumb.setAttribute('aria-current', 'page');
      } else {
        thumb.removeAttribute('aria-current');
      }
    });
    
    // Announce to screen readers
    const announcement = `Image ${this.currentImageIndex + 1} of ${this.images.length}: ${image.alt}`;
    const status = document.querySelector('[role=\"status\"]');
    if (status) status.textContent = announcement;
  }
  
  setupFocusTrap() {
    // Keep focus within lightbox
    const focusableElements = this.lightbox.querySelectorAll(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex=\"-1\"])'
    );
    
    const firstElement = focusableElements[0];
    const lastElement = focusableElements[focusableElements.length - 1];
    
    this.lightbox.addEventListener('keydown', (e) => {
      if (e.key !== 'Tab') return;
      
      if (e.shiftKey) {
        // Shift+Tab on first element → go to last
        if (document.activeElement === firstElement) {
          e.preventDefault();
          lastElement.focus();
        }
      } else {
        // Tab on last element → go to first
        if (document.activeElement === lastElement) {
          e.preventDefault();
          firstElement.focus();
        }
      }
    });
  }
}

// Initialize
document.addEventListener('DOMContentLoaded', () => {
  new ImageGallery();
});
</script>
```

**Key Accessibility Features:

1. **Focus Management**: Saves previous focus, restores on close
2. **Focus Trap**: Tab stays within lightbox
3. **Keyboard Navigation**: Arrows, Home/End, Escape
4. **Screen Reader**: ARIA labels, live region updates
5. **ARIA Modal**: aria-modal, aria-hidden for screen readers
6. **Visible Focus Indicator**: Yellow outline for keyboard users
7. **Semantic HTML**: figure/figcaption for images

**Why this matters:
- Keyboard-only users can navigate gallery
- Screen reader users hear 'Image 3 of 6'
- Mobile users see clear navigation
- Complex interaction remains accessible**"

**Common mistakes:**
❌ No focus trap (Tab escapes lightbox)
❌ No keyboard navigation
❌ No focus restoration on close
❌ Images without alt text
❌ No aria-hidden for backdrop
✅ Full keyboard support
✅ Proper focus management
✅ Screen reader announcements

**Follow-up questions:**
- "How would you handle image loading/lazy loading?"
- "What about touch gestures on mobile?"
- "How would you handle very large images?"

**Score:** 10/10 because production-ready, accessible, and comprehensive.

---

## ⚡ PART 4: QUICK REFERENCE

### The 30-Second Pitch

**"HTML is the skeleton of websites—it organizes content using semantic elements like `<header>`, `<nav>`, `<main>`, `<article>`, and `<footer>`. Good HTML structure makes pages accessible for disabled users, discoverable by Google, and maintainable for developers. Always use semantic HTML over generic divs, ensure forms are properly labeled, and include ARIA attributes when semantics aren't available. This foundation makes everything else (CSS, JavaScript) work better."**

### The 3 Critical Things

1. **Semantic HTML**: Use meaningful tags (`<nav>`, `<article>`, `<button>`) not just `<div>`. This helps screen readers, search engines, and other developers understand structure.

2. **Accessibility**: Proper labels, ARIA attributes, and keyboard navigation ensure your site works for the 15% of people with disabilities. It's also the law (ADA, AODA, Equality Act).

3. **Form Structure**: Use `<label>`, `<form>`, `<fieldset>`, proper input types, and validation. Forms are where users interact—get them right.

### Quick Answers

| Question | 30-Second Answer |
|----------|-----------------|
| What is semantic HTML? | Tags with meaning (`<nav>`, `<article>`) vs generic divs. Helps screen readers and Google understand structure. |
| Why is accessibility important? | 15% of people have disabilities. It's required by law. Good accessibility benefits everyone. |
| What's the difference between `<button>` and `<div onclick>`? | `<button>` is keyboard-accessible by default. `<div onclick>` requires JavaScript and ARIA to work. |
| How do I label form inputs? | Use `<label for="id">` connected to `<input id="id">`. Placeholder ≠ label. |
| What's ARIA? | Accessible Rich Internet Applications. Attributes that tell screen readers about custom interactive elements. |
| Why use form `type` attribute? | `type="email"` validates format, shows right keyboard on mobile, and helps screen readers. |
| What's Shadow DOM? | Encapsulates styles so they don't leak out. Used in Web Components for isolation. |
| How do I make a table accessible? | Use `<thead>`, `<tbody>`, `scope="col"` on headers, and `<caption>`. |
| Do I need to test with screen readers? | Yes. 15% of users use them. Use NVDA (Windows) or VoiceOver (Mac). |
| What's the heading hierarchy? | One `<h1>` per page, then `<h2>`, `<h3>` in order. Screen readers depend on it. |

### The Gotchas

- **Gotcha #1 - Missing Labels**: Using placeholder instead of `<label>`. Screen readers and form autofill fail.
- **Gotcha #2 - Divs for Buttons**: Using `<div onclick>` instead of `<button>`. Not keyboard accessible.
- **Gotcha #3 - Skip Heading Levels**: Using h1 → h3 (skipping h2). Breaks screen reader navigation.
- **Gotcha #4 - No Alt Text**: Images without alt text. Screen reader users see nothing.
- **Gotcha #5 - Color-Only Info**: Using color alone to show status. Colorblind users miss it.
- **Gotcha #6 - Keyboard Trap**: Focus gets stuck in a component. User can't escape.
- **Gotcha #7 - Missing Viewport Meta**: Mobile layout breaks. Users have to zoom.
- **Gotcha #8 - Multiple H1s**: Google confused about main topic. SEO ranking drops.

### Interview Strategy

**If you're strong:**
- Discuss Web Components vs React trade-offs
- Explain focus management in modal dialogs
- Talk about ARIA patterns you've implemented
- Mention accessibility audit tools you've used
- Discuss how semantic HTML improves SEO

**If you're medium:**
- Explain semantic elements clearly
- Show understanding of accessibility
- Discuss form validation approaches
- Mention ARIA when relevant
- Know common gotchas

**If you're weak:**
- Focus on: What semantic HTML is and why it matters
- Explain three semantic elements: `<nav>`, `<main>`, `<footer>`
- Talk about labels in forms
- Mention accessibility for disabled users
- Say you'd like to learn Web Components

### Checklist

- [ ] Can explain semantic HTML in 30 seconds
- [ ] Know 8+ semantic elements by heart
- [ ] Understand accessibility basics (WCAG 2.1 level AA)
- [ ] Can build an accessible form from scratch
- [ ] Know form validation (HTML5 + custom)
- [ ] Understand ARIA roles and attributes
- [ ] Know heading hierarchy rules
- [ ] Can explain Web Components
- [ ] Know the gotchas and common mistakes
- [ ] Can test with keyboard and screen reader

---

## 🧠 PART 5: QUIZ - RAPID FIRE (20 Questions)

### Q1: What does the `<header>` semantic element represent?

- A) The top of the page where the logo goes
- B) A top-level heading for content (use `<h1>` instead)
- C) A container that groups intro or navigation content
- D) Required for every page

**Correct Answer:** C - The `<header>` is a semantic element that groups introductory content like logo, site title, or navigation. It's NOT required and can appear multiple times (once per section).

---

### Q2: Why should you use `<label>` elements in forms?

- A) For styling purposes only
- B) To connect text with form inputs so screen readers know what each field is
- C) As an alternative to placeholder text
- D) Only for accessibility, not necessary otherwise

**Correct Answer:** B - Labels connect text with inputs via `for` and `id` attributes. Screen readers, form autofill, and keyboard users all depend on proper labels. Placeholder ≠ label.

---

### Q3: What is ARIA?

- A) A JavaScript framework
- B) A styling system for accessibility
- C) Accessible Rich Internet Applications - attributes that enhance accessibility
- D) A type of HTML element

**Correct Answer:** C - ARIA is a set of attributes (aria-label, aria-hidden, aria-live, etc.) that provide accessibility information when semantic HTML isn't available. Used for custom interactive components.

---

### Q4: How many `<h1>` elements should a page have?

- A) One (recommended best practice)
- B) As many as needed
- C) None (use CSS for sizing)
- D) At least 3

**Correct Answer:** A - Best practice is one `<h1>` per page representing the main topic. Multiple h1s confuse screen readers and search engines. If you want text to "look like h1," use CSS styling on semantic elements.

---

### Q5: What does the `<main>` element do?

- A) Makes the content the primary focus
- B) Required for valid HTML
- C) Defines the main content area (unique per page)
- D) The same as `<body>`

**Correct Answer:** C - `<main>` wraps the page's unique content. One `<main>` per page. Search engines and screen reader users use it to jump to main content. Header/footer/nav go outside `<main>`.

---

### Q6: Which form input type provides email validation?

- A) `type="text"`
- B) `type="email"`
- C) `type="validate-email"`
- D) No HTML5 type validates email

**Correct Answer:** B - `type="email"` validates email format, shows email keyboard on mobile, and helps accessibility tools. Still validate on the server—never trust client-side only.

---

### Q7: What does Web Components' Shadow DOM do?

- A) Makes components invisible
- B) Encapsulates styles and structure (CSS doesn't leak out)
- C) Stores component data
- D) Creates a hidden copy of the component

**Correct Answer:** B - Shadow DOM isolates styles so they don't affect the page and page styles don't affect the component. Critical for reusable components that need to work anywhere.

---

### Q8: When should you use `<aside>`?

- A) For any content on the side of the page
- B) For content tangential to main content (sidebars, related links, ads)
- C) Always on the right side
- D) Never, use divs instead

**Correct Answer:** B - `<aside>` is for supplementary content related to but not central to the main content. Sidebars, related articles, ads. NOT just "content on the side."

---

### Q9: How do you test if your site is accessible?

- A) Run an automated tool (Axe, Lighthouse)
- B) Test with keyboard only (no mouse)
- C) Test with screen reader (NVDA, VoiceOver)
- D) All of the above

**Correct Answer:** D - Use automated tools to catch common issues, but also manual testing: keyboard-only and with screen readers. Tools catch ~30% of issues. Manual testing catches what automated misses.

---

### Q10: What's the purpose of `scope="col"` on table headers?

- A) Makes the column wider
- B) Tells screen readers which header applies to each column
- C) Sorts the table
- D) Required for styling tables

**Correct Answer:** B - `scope="col"` (and `scope="row"`) tells screen readers the relationship between headers and data cells. Screen reader says "Name: Alice" (header + cell value) instead of just "Alice."

---

### Q11: Why should form inputs have the `required` attribute?

- A) Prevents form submission without browser validation
- B) Shows users which fields must be filled
- C) Helps accessibility (aria-required suggested)
- D) All of the above

**Correct Answer:** D - `required` blocks submission, shows visual indicator to users, helps screen readers, and improves UX. Still validate on server (user can remove attribute with DevTools).

---

### Q12: What does `aria-label` do?

- A) Same as the `title` attribute
- B) Provides a text description for screen readers
- C) Shows a tooltip on hover
- D) Labels form inputs

**Correct Answer:** B - `aria-label` provides accessibility text for screen readers. Different from title (tooltip) or label (form elements). Use when you can't use semantic HTML or need extra context.

---

### Q13: What's the difference between `<section>` and `<article>`?

- A) `<section>` groups related content, `<article>` is self-contained
- B) `<article>` is bigger than `<section>`
- C) `<section>` is for navigation only
- D) They're identical

**Correct Answer:** A - `<section>` groups thematically related content. `<article>` is self-contained (could be republished elsewhere). Blog post = `<article>`. Blog post chapters = `<section>`.

---

### Q14: How do you hide content from sighted users but show it to screen readers?

- A) `display: none;` (hides from both)
- B) `visibility: hidden;` (hides from both)
- C) `.sr-only` class with specific CSS positioning
- D) `aria-hidden="true"` (hides from both)

**Correct Answer:** C - `.sr-only` (screen reader only) uses absolute positioning to move content off-screen visually but keeps it available to screen readers. Used for additional context.

---

### Q15: What input types does HTML5 provide for validation?

- A) Only text, email, password
- B) email, number, date, url, color, range, etc.
- C) Unlimited custom types
- D) HTML5 doesn't validate input types

**Correct Answer:** B - HTML5 provides type="email", type="number", type="date", type="url", type="color", type="range", type="tel", type="search", and more. Each provides validation and appropriate mobile keyboards.

---

### Q16: What does `aria-expanded` indicate?

- A) How much of the page is visible
- B) Whether an element (like a menu) is open/closed
- C) The width of an expandable element
- D) Not a real ARIA attribute

**Correct Answer:** B - `aria-expanded="true/false"` tells screen readers if a toggleable element (dropdown, accordion, menu) is expanded or collapsed. Updates when toggled.

---

### Q17: Why is `<form>` better than a `<div>` for forms?

- A) It looks better
- B) Semantic meaning, proper form submission, password managers work better
- C) No difference functionally
- D) Form attributes are outdated

**Correct Answer:** B - `<form>` is semantic (tells browsers this is a form), enables form submission, helps password managers recognize login forms, and provides better accessibility. Divs need manual JavaScript.

---

### Q18: What's the purpose of the `<meta name="viewport">` tag?

- A) Makes the site look good on desktop only
- B) Enables mobile responsive design
- C) Shows the site in a mobile browser window
- D) No longer needed in modern HTML

**Correct Answer:** B - `<meta name="viewport" content="width=device-width, initial-scale=1.0">` tells browsers to render at device width and not zoom. Without it, mobile sites are zoomed out/unreadable.

---

### Q19: When should you use `role` attribute in HTML?

- A) Always, for every element
- B) Never, semantic HTML is enough
- C) When semantic HTML doesn't exist for your component
- D) Only for testing

**Correct Answer:** C - Use semantic HTML first (`<button>`, `<nav>`, `<article>`). Use `role` only when you have custom components that need accessibility (like `role="dialog"` for custom modal).

---

### Q20: What's best practice for form error messages?

- A) Show in JavaScript alert popup
- B) Turn input red and rely on color alone
- C) Link error message with `aria-describedby` and show near input
- D) Only validate on server, don't show client-side errors

**Correct Answer:** C - Place error message near the input, link with `aria-describedby`, show text + color (not color alone), and announce to screen readers with role="alert". Users understand what's wrong without confusion.

---

### Scoring

- **18-20: Expert** — You understand HTML deeply and can build accessible components
- **15-17: Advanced** — Strong HTML and accessibility knowledge; minor gaps
- **12-14: Intermediate** — Good foundation; review semantic HTML and ARIA
- **9-11: Beginner** — Core concepts understood; practice with real components
- **<9: Review needed** — Study PART 1 & 2 thoroughly before interviews

---

## 🛠 PART 6: MACHINE CODING CHALLENGE

### The Challenge

**Build an accessible todo list application using semantic HTML, proper form structure, and ARIA attributes.**

Requirements (vague on purpose, like real interviews):
- Users can add todos
- Mark todos complete/incomplete
- Delete todos
- Accessible to keyboard and screen reader users
- Responsive design
- No framework (vanilla HTML/CSS/JS)

### Clarifying Questions (Ask These in Interview)

- Should todos persist when page reloads? (LocalStorage? Yes, add it)
- Can users edit todo text? (Yes, but that's bonus, not required)
- Should there be a "clear completed" button? (Yes, good idea)
- Do todos have categories? (Not for MVP, keep simple)
- What about animations? (Bonus, not required)

### Solution Guide

#### Step 1: Semantic HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Accessible Todo List</title>
</head>
<body>
  <main>
    <header>
      <h1>My Todo List</h1>
      <p>Keep track of your tasks</p>
    </header>
    
    <!-- Input Form -->
    <form id="todo-form" aria-label="Add new todo">
      <label for="todo-input">Add a new todo:</label>
      
      <input 
        id="todo-input"
        type="text"
        placeholder="e.g., Buy groceries"
        required
        aria-required="true"
        aria-describedby="input-help"
      >
      
      <small id="input-help">Press Enter or click Add button</small>
      
      <button type="submit">Add Todo</button>
    </form>
    
    <!-- Stats -->
    <div aria-live="polite" role="status" id="stats">
      <p>0 todos, 0 completed</p>
    </div>
    
    <!-- Todo List -->
    <section aria-label="Todo list">
      <ul id="todo-list" role="list">
        <!-- Todos added here -->
      </ul>
    </section>
    
    <!-- Clear Completed -->
    <button 
      id="clear-completed" 
      aria-label="Remove all completed todos"
      disabled
    >
      Clear Completed
    </button>
  </main>
  
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      padding: 20px;
    }
    
    main {
      max-width: 600px;
      margin: 0 auto;
      background: white;
      border-radius: 8px;
      padding: 30px;
      box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
    }
    
    h1 {
      color: #333;
      margin-bottom: 10px;
    }
    
    header p {
      color: #999;
      margin-bottom: 30px;
    }
    
    form {
      display: flex;
      gap: 10px;
      margin-bottom: 30px;
      flex-wrap: wrap;
    }
    
    label {
      display: block;
      width: 100%;
      font-weight: 500;
      color: #333;
      margin-bottom: 5px;
    }
    
    input[type="text"] {
      flex: 1;
      min-width: 250px;
      padding: 10px 15px;
      border: 2px solid #ddd;
      border-radius: 4px;
      font-size: 16px;
      transition: border-color 0.3s;
    }
    
    input[type="text"]:focus {
      outline: none;
      border-color: #667eea;
    }
    
    button {
      padding: 10px 20px;
      background: #667eea;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 16px;
      transition: background 0.3s;
    }
    
    button:hover:not(:disabled) {
      background: #5568d3;
    }
    
    button:focus-visible {
      outline: 3px solid #667eea;
      outline-offset: 2px;
    }
    
    button:disabled {
      background: #ddd;
      cursor: not-allowed;
    }
    
    small {
      width: 100%;
      color: #999;
      font-size: 12px;
    }
    
    #stats {
      background: #f5f5f5;
      padding: 15px;
      border-radius: 4px;
      margin-bottom: 20px;
      color: #666;
    }
    
    #todo-list {
      list-style: none;
      margin-bottom: 20px;
    }
    
    .todo-item {
      display: flex;
      align-items: center;
      gap: 15px;
      padding: 15px;
      border: 1px solid #eee;
      border-radius: 4px;
      margin-bottom: 10px;
      transition: all 0.3s;
    }
    
    .todo-item:hover {
      background: #f9f9f9;
      border-color: #ddd;
    }
    
    .todo-item.completed {
      opacity: 0.6;
      background: #f9f9f9;
    }
    
    .todo-item.completed .todo-text {
      text-decoration: line-through;
      color: #999;
    }
    
    .todo-checkbox {
      width: 24px;
      height: 24px;
      cursor: pointer;
      accent-color: #667eea;
    }
    
    .todo-text {
      flex: 1;
      color: #333;
      word-break: break-word;
    }
    
    .todo-delete {
      background: #e74c3c;
      padding: 8px 12px;
      font-size: 14px;
    }
    
    .todo-delete:hover {
      background: #c0392b;
    }
    
    .sr-only {
      position: absolute;
      width: 1px;
      height: 1px;
      overflow: hidden;
      clip: rect(0,0,0,0);
    }
    
    @media (max-width: 600px) {
      main {
        padding: 20px;
      }
      
      form {
        flex-direction: column;
      }
      
      input[type="text"] {
        min-width: unset;
      }
    }
  </style>
  
  <script>
    class TodoApp {
      constructor() {
        this.todos = this.loadFromStorage() || [];
        this.form = document.getElementById('todo-form');
        this.input = document.getElementById('todo-input');
        this.list = document.getElementById('todo-list');
        this.stats = document.getElementById('stats');
        this.clearBtn = document.getElementById('clear-completed');
        
        this.setupEventListeners();
        this.render();
      }
      
      setupEventListeners() {
        this.form.addEventListener('submit', (e) => {
          e.preventDefault();
          this.addTodo(this.input.value);
          this.input.value = '';
          this.input.focus();
        });
        
        this.clearBtn.addEventListener('click', () => {
          this.clearCompleted();
        });
      }
      
      addTodo(text) {
        if (!text.trim()) return;
        
        const todo = {
          id: Date.now(),
          text: text,
          completed: false
        };
        
        this.todos.push(todo);
        this.saveToStorage();
        this.render();
        
        // Announce to screen readers
        this.announce(`${text} added to list`);
      }
      
      toggleTodo(id) {
        const todo = this.todos.find(t => t.id === id);
        if (todo) {
          todo.completed = !todo.completed;
          this.saveToStorage();
          this.render();
          
          const status = todo.completed ? 'marked complete' : 'marked incomplete';
          this.announce(`${todo.text} ${status}`);
        }
      }
      
      deleteTodo(id) {
        const todo = this.todos.find(t => t.id === id);
        if (todo) {
          this.todos = this.todos.filter(t => t.id !== id);
          this.saveToStorage();
          this.render();
          
          this.announce(`${todo.text} deleted`);
        }
      }
      
      clearCompleted() {
        const completedCount = this.todos.filter(t => t.completed).length;
        this.todos = this.todos.filter(t => !t.completed);
        this.saveToStorage();
        this.render();
        
        this.announce(`${completedCount} completed todos deleted`);
      }
      
      render() {
        // Clear list
        this.list.innerHTML = '';
        
        // Add todos
        this.todos.forEach(todo => {
          const li = document.createElement('li');
          li.className = `todo-item ${todo.completed ? 'completed' : ''}`;
          li.setAttribute('role', 'listitem');
          
          li.innerHTML = `
            <input 
              type="checkbox" 
              class="todo-checkbox"
              ${todo.completed ? 'checked' : ''}
              aria-label="Mark '${todo.text}' complete"
              data-id="${todo.id}"
            >
            <span class="todo-text">${this.escapeHTML(todo.text)}</span>
            <button 
              class="todo-delete" 
              data-id="${todo.id}"
              aria-label="Delete '${todo.text}'"
            >
              Delete
            </button>
          `;
          
          // Toggle on checkbox
          li.querySelector('.todo-checkbox').addEventListener('change', () => {
            this.toggleTodo(todo.id);
          });
          
          // Delete button
          li.querySelector('.todo-delete').addEventListener('click', () => {
            this.deleteTodo(todo.id);
          });
          
          this.list.appendChild(li);
        });
        
        // Update stats
        const total = this.todos.length;
        const completed = this.todos.filter(t => t.completed).length;
        this.stats.querySelector('p').textContent = 
          `${total} todo${total !== 1 ? 's' : ''}, ${completed} completed`;
        
        // Enable/disable clear button
        this.clearBtn.disabled = completed === 0;
      }
      
      saveToStorage() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
      
      loadFromStorage() {
        const stored = localStorage.getItem('todos');
        return stored ? JSON.parse(stored) : null;
      }
      
      announce(message) {
        const announcement = document.createElement('div');
        announcement.className = 'sr-only';
        announcement.setAttribute('role', 'status');
        announcement.setAttribute('aria-live', 'polite');
        announcement.textContent = message;
        document.body.appendChild(announcement);
        
        setTimeout(() => announcement.remove(), 1000);
      }
      
      escapeHTML(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
      }
    }
    
    // Initialize app
    document.addEventListener('DOMContentLoaded', () => {
      new TodoApp();
    });
  </script>
</body>
</html>
```

#### Step 2: Advanced Features

**Add Edit Functionality:**
```javascript
editTodo(id, newText) {
  const todo = this.todos.find(t => t.id === id);
  if (todo) {
    todo.text = newText;
    this.saveToStorage();
    this.render();
    this.announce(`${todo.text} updated`);
  }
}
```

**Add Drag & Drop:**
```javascript
// Make list sortable (bonus)
// Use HTML5 drag and drop API or library like Sortable.js
```

**Add Categories:**
```javascript
// Add category field to todos
// Filter by category
// Display categories as tabs
```

### Evaluation Criteria

| Criterion | Points | What Interviewer Looks For |
|-----------|--------|---------------------------|
| **Functionality** | /10 | Add, complete, delete todos work. LocalStorage persists. |
| **Semantic HTML** | /10 | Proper `<form>`, `<label>`, `<ul>/<li>`, `<button>`. |
| **Accessibility** | /10 | Keyboard navigation, ARIA labels, screen reader support. |
| **Code Quality** | /10 | Clean structure, no console errors, proper naming. |
| **Responsive Design** | /10 | Works on mobile, tablet, desktop. |
| **Total** | /50 | |

---

### Common Mistakes to Avoid

❌ **Using `<div>` for buttons**: Not keyboard accessible
```javascript
<div onclick="addTodo()">Add</div> // Wrong!
<button onclick="addTodo()">Add</button> // Right!
```

❌ **Missing labels on inputs**: Screen readers can't find field purpose
```html
<input type="text"> <!-- Wrong! -->
<label for="input">Todo:</label>
<input id="input" type="text"> <!-- Right! -->
```

❌ **No persistent storage**: Data lost on refresh
```javascript
this.saveToStorage(); // Always do this
```

❌ **No focus management**: After adding todo, focus stays in input
```javascript
this.input.focus(); // Reset focus so user can keep typing
```

❌ **Not announcing changes to screen readers**: Users don't know what happened
```javascript
this.announce(`${text} added to list`); // Always announce
```

### Follow-up Questions Interviewer Might Ask

- "How would you add due dates to todos?"
- "How would you implement categories/filtering?"
- "What if there were 10,000 todos? Performance?"
- "How would you make todos editable inline?"
- "What about syncing with a backend API?"
- "How would you add drag-and-drop reordering?"
- "Can you implement undo/redo?"

### Time Estimate

- Step 1 (Basic): 30 minutes
- Step 2 (Features): 45 minutes
- Testing: 15 minutes
- **Total: 45-60 minutes**

---

## ✅ PART 7: MASTERY CHECKLIST & NEXT STEPS

### Mastery Checklist

Before you claim mastery of HTML, verify:

#### Semantic HTML
- [ ] Can name 10+ semantic elements (`<nav>`, `<article>`, `<section>`, etc.)
- [ ] Understand when to use `<article>` vs `<section>`
- [ ] Know what goes in `<header>`, `<main>`, `<footer>`
- [ ] Understand `<aside>` usage (not just "sidebar")
- [ ] Can build page structure without using `<div>` everywhere

#### Forms
- [ ] Can build accessible form from scratch
- [ ] Know `<label>` connection (for/id) is critical
- [ ] Understand HTML5 input types and validation
- [ ] Know difference between `required`, `pattern`, `minlength`
- [ ] Can implement form error handling with `aria-describedby`
- [ ] Understand form submission and validation flow
- [ ] Know `<fieldset>` and `<legend>` usage

#### Accessibility
- [ ] Can explain why accessibility matters (law + users)
- [ ] Know WCAG 2.1 Level AA basics
- [ ] Understand `role`, `aria-label`, `aria-describedby`, `aria-hidden`
- [ ] Can test with keyboard only (Tab, Enter, Arrows)
- [ ] Have used screen reader (NVDA, VoiceOver)
- [ ] Know `aria-live` for announcements
- [ ] Can implement focus management (modals, etc.)
- [ ] Know `tabindex` and focus trap patterns

#### Heading Hierarchy
- [ ] One `<h1>` per page is best practice
- [ ] Headings must be sequential (h1 → h2 → h3, no skips)
- [ ] Can explain why hierarchy matters (screen readers, SEO, outline)
- [ ] Would never skip a level intentionally

#### SEO
- [ ] Know importance of `<title>` and `<meta description>`
- [ ] Understand semantic HTML helps Google
- [ ] Know Open Graph meta tags
- [ ] Understand structured data (JSON-LD)
- [ ] Know heading hierarchy affects SEO

#### Web Components
- [ ] Understand Custom Elements API
- [ ] Know what Shadow DOM is and why it matters
- [ ] Know slots and how they work
- [ ] Understand when to use Web Components vs framework

#### Best Practices
- [ ] Can explain why semantic > div-soup
- [ ] Know DOCTYPE is required
- [ ] Understand `<meta charset>` must be first in `<head>`
- [ ] Know viewport meta tag is critical
- [ ] Can spot accessibility issues in code
- [ ] Know common gotchas (heading skips, missing labels, color-only info)

### How to Practice

1. **Build From Scratch**: Create a blog, portfolio, or document using semantic HTML. No framework.

2. **Audit Existing Sites**: Check your favorite sites with accessibility tools (Axe DevTools, Lighthouse).

3. **Test with Keyboard**: Use website with keyboard only (no mouse). Can you navigate everything?

4. **Test with Screen Reader**: Use NVDA (Windows) or VoiceOver (Mac). Does it make sense?

5. **Read Standards**: Glance through WCAG 2.1 Level AA guidelines (www.w3.org/WAI/WCAG21/quickref/).

6. **Interview Practice**: Answer the 10 interview questions above. Can you explain why, not just how?

### Timeline to Mastery

- **Week 1**: Learn basics (Parts 0, 1, 2) + build todo list challenge
- **Week 2**: Learn forms, validation, accessibility (Part 1)
- **Week 3**: Practice interview questions (Part 3) + build form challenges
- **Week 4**: Learn advanced patterns (Web Components, modals, tables)
- **Week 5-6**: Interview preparation + mock interviews

**Total: 5-6 weeks to solid mastery**

### Next Topics After HTML

After mastering HTML, move to:

1. **CSS (1.3)** — How to style semantic HTML
   - Box model, Flexbox, Grid
   - Responsive design, Mobile-first
   - Accessibility in CSS (focus, color contrast)

2. **JavaScript (1.4)** — Make HTML interactive
   - DOM manipulation
   - Event handling
   - Form validation and submission

### Real-World Application

In a real job (Google, Meta, Netflix):

- You'll **always** write semantic HTML first
- **50% of bugs** come from poor HTML structure (layout, a11y)
- **SEO matters**: Company revenue depends on search traffic
- **Accessibility isn't optional**: Legal requirement, also the right thing
- **Code reviews** focus on HTML semantics
- **Mobile users** depend on proper HTML structure

### Interview Tips

1. **Always explain why**: "I use `<button>` because it's keyboard-accessible by default"
2. **Show accessibility thinking**: Mention screen readers, keyboard navigation, ARIA
3. **Know the gotchas**: "Many developers use `<div onclick>` but that's not keyboard accessible"
4. **Ask clarifying questions**: "Should todos persist?" (shows experience)
5. **Test as you build**: "I'd test this with keyboard and VoiceOver"
6. **Think about users**: "15% of people have disabilities—we can't ignore them"

### Resources

- **WCAG 2.1**: https://www.w3.org/WAI/WCAG21/quickref/
- **MDN HTML**: https://developer.mozilla.org/en-US/docs/Web/HTML
- **WebAIM Accessibility**: https://webaim.org/
- **Semantic HTML**: https://developer.mozilla.org/en-US/docs/Glossary/Semantics
- **ARIA Authoring Practices**: https://www.w3.org/WAI/ARIA/apg/
- **Screen Readers**: NVDA (free), VoiceOver (built-in Mac), JAWS (premium)

---

## 🎓 Final Thoughts

HTML is **foundational**. It's the skeleton that everything else hangs on. Get HTML right, and:

- ✅ Your site works for disabled users (legally required)
- ✅ Google understands and ranks your content
- ✅ Your team understands your code structure
- ✅ Styling and JavaScript become much easier
- ✅ Performance improves naturally

**In interviews**, demonstrate:
1. Deep understanding of semantics
2. Commitment to accessibility
3. Real-world problem solving
4. Clean code and best practices

Most developers skip HTML because it seems "easy." They're wrong. **HTML is where the architecture happens.** Master it, and you'll stand out.

---

**You've completed the comprehensive HTML Skeleton guide. You're ready to master CSS next.** 🚀

