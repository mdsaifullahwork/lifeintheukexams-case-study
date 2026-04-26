# Life in the UK Exam – WordPress Plugin

Technical documentation for the custom exam plugin.

| Field | Details |
| --- | --- |
| **Plugin name** | Life in the UK Exam |
| **Version** | 1.2 |
| **Author** | Md Saifullah |
| **Last updated** | October 11, 2025 |

**Description:** The Life in the UK Exam plugin provides an interactive exam widget for WordPress sites through a shortcode. Visitors can take realistic practice exams for the official Life in the UK test. Each exam has exactly **24 questions**, supports **single and multiple-choice** answers, includes a **45-minute** timer, navigation controls, progress tracking, a results summary, and works with Elementor or the default WordPress editor.

---

## Table of contents

1. [Overview](#1-overview)
2. [Features](#2-features)
3. [Requirements](#3-requirements)
4. [Installation](#4-installation)
5. [Setting up Advanced Custom Fields (ACF)](#5-setting-up-advanced-custom-fields-acf)
6. [Creating exam posts](#6-creating-exam-posts)
7. [Embedding exams in pages](#7-embedding-exams-in-pages)
8. [JSON question format](#8-json-question-format)
9. [Testing the exam](#9-testing-the-exam)
10. [Performance considerations](#10-performance-considerations)
11. [Troubleshooting](#11-troubleshooting)
12. [Support and enhancements](#12-support-and-enhancements)

---

## 1. Overview

The Life in the UK Exam plugin allows WordPress administrators to create and manage practice exams for users preparing for the Life in the UK test. It adds a custom post type **Exams** (`liuk_exam`); each exam stores its questions in JSON inside an Advanced Custom Fields (ACF) field `liuk_questions`.

Users take the exam via a shortcode on any page, which shows a fully interactive, responsive exam UI.

---

## 2. Features

- 24-question interactive exam
- Single or multiple-choice support
- 45-minute countdown timer
- Save progress and review answers
- Score, correct answers, and explanations
- Responsive, mobile-friendly layout
- Works with Elementor and the classic editor
- Lightweight and performance-oriented

---

## 3. Requirements

- WordPress **5.0** or later
- PHP **7.4** or later
- **Advanced Custom Fields (ACF)** installed and active
- Optional: **Elementor** for page building
- A modern browser (Chrome, Edge, Firefox, Safari)

---

## 4. Installation

1. In your WordPress install, open `wp-content/plugins/`.
2. Create a folder named `life-in-the-uk-exam`.
3. Inside it, add the main file `life-in-uk-exam.php` and add the full plugin code.
4. In the admin, go to **Plugins → Installed Plugins**.
5. Activate **Life in the UK Exam**.

---

## 5. Setting up Advanced Custom Fields (ACF)

1. Install and activate **Advanced Custom Fields**.
2. Create a new field group: **Life in the UK Exam Questions**.
3. Add a field with:

   | Property | Value |
   | --- | --- |
   | Field label | Questions (JSON) |
   | Field name | `liuk_questions` |
   | Field type | Textarea |
   | Instructions | Enter exactly 24 questions in valid JSON. |
   | Location | Post type = **Exams** |

   This field holds the questions as JSON.

### Alternative: import a field group

- Get the ACF field group JSON from the developer.
- Go to **Custom Fields → Tools → Import Field Groups**.
- Upload the JSON and import, then confirm **Exam Questions** is assigned to the **Exams** post type.

---

## 6. Creating exam posts

1. Go to **Exams → Add New**.
2. Set the exam title (e.g. Practice Test 1).
3. In **Questions (JSON)**, paste a valid JSON array of **24** questions.
4. **Publish**.

Each object should look like this:

```json
{
  "text": "What is the capital of the UK?",
  "options": ["London", "Paris", "Rome", "Berlin"],
  "correct": [0],
  "explanation": "London is the capital of the United Kingdom."
}
```

---

## 7. Embedding exams in pages

Use the shortcode:

```text
[life_in_uk_exam post_id="123" title="Practice Test" pass_threshold="0.75"]
```

### Parameters

| Parameter | Description |
| --- | --- |
| `post_id` | ID of the **Exams** post |
| `title` | Optional heading above the exam |
| `pass_threshold` | Optional; minimum pass ratio (default `0.75`) |

**Example:**

```text
[life_in_uk_exam post_id="456" title="Life in the UK Mock Test" pass_threshold="0.80"]
```

Works in Elementor text widgets, the block editor, and the classic editor.

---

## 8. JSON question format

Each exam must include **24** questions in a single array:

```json
[
  {
    "text": "Which of the following are UK capital cities?",
    "options": ["London", "Edinburgh", "Cardiff", "Dublin"],
    "correct": [0, 1, 2],
    "explanation": "London, Edinburgh, and Cardiff are the capitals of England, Scotland, and Wales."
  }
]
```

**Rules:**

- `correct` is always an array (e.g. `[0]` for one answer, `[0, 2]` for multiple).
- Every question needs `text`, `options`, `correct`, and `explanation`.
- JSON must be valid: no trailing commas, correct quoting.

---

## 9. Testing the exam

After you publish an exam:

1. Put the shortcode on a test page and view it on the front end.
2. Check that:
   - The timer runs from **45 minutes** downward.
   - You can move between questions.
   - Right/wrong answers are clear after submission.
   - The results view shows score and explanations.
3. Confirm all **24** questions load.

---

## 10. Performance considerations

- Minimal JavaScript and lean CSS.
- Questions load from the stored JSON (no external API for the test itself).
- Pagination and the timer run in the browser.
- Suitable for typical shared hosting.

---

## 11. Troubleshooting

| Issue | Possible cause | What to do |
| --- | --- | --- |
| Exam not showing | Wrong or missing `post_id` in the shortcode | Use the real ID of a published **Exams** post |
| Questions not loading | Invalid JSON | Validate JSON before saving (e.g. with a JSON linter) |
| Timer not starting | JS conflict | Check the browser console; try disabling other plugins to isolate |
| Score not shown | Bad or missing `correct` values | Ensure each question has a `correct` array as required |

---

## 12. Support and enhancements

For bug reports, feature ideas, or development help, contact **Md Saifullah**.

**Possible future work:**

- Logged-in user progress and score history
- Leaderboards
- Deeper analytics
- Configurable timer length
