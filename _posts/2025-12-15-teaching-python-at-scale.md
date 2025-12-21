---
layout: post
title: "Teaching Python to 100+ Students: What Actually Works"
date: 2025-12-15
categories: [teaching]
tags: [python, education, programming, universidad-nacional]
excerpt: "After years teaching programming courses to large cohorts at Universidad Nacional, I've learned that teaching Python at scale requires different strategies than small classes. Here's what works."
---

Since 2016, I've taught Python programming and Object-Oriented Programming to hundreds of students at Universidad Nacional de Colombia and other universities. Teaching cohorts of 100+ students simultaneously is different from teaching 20. Here's what I've learned.

## The Challenge: Scale Without Losing Quality

With 100+ students, you can't:
- Give individualized code reviews to everyone
- Debug every student's environment issues
- Answer every question in real-time during lectures

But students still need:
- Clear explanations of concepts
- Hands-on practice
- Timely feedback
- Support when they're stuck

The solution isn't working harder—it's working smarter.

## What Actually Works

### 1. Everything on GitHub from Day 1

All course materials live in public GitHub repositories:
- Lecture notebooks (Jupyter)
- Exercise sets with solutions
- Example projects
- Assignment templates

**Why this works:**
- Students can access materials anytime, anywhere
- Version control teaches professional practices
- Students can submit assignments via Pull Requests
- Other students can learn from public examples

My repositories have gained 60-114 stars each, with students from other universities using them too.

**Example repos:** [pdc_unal_clase1-6](https://github.com/fegonzalez7)

### 2. Live Coding > Slides

I abandoned PowerPoint presentations. Instead:
- Write code live in Jupyter notebooks
- Make mistakes intentionally, debug them publicly
- Show the thought process, not just the solution

**Why this works:**
- Students see that programming is iterative
- They learn debugging is normal
- Real-time questions integrate naturally

### 3. Peer Learning Through Code Reviews

With 100+ students, I can't review every assignment individually. Solution: **structured peer reviews**.

Process:
1. Students submit code via GitHub Pull Request
2. Each student is assigned 2 peer submissions to review
3. Reviews use a rubric (code clarity, documentation, correctness)
4. I review a random sample (20%) and all disputed cases

**Why this works:**
- Students learn by reading others' code
- Scales without sacrificing feedback
- Teaches professional collaboration skills

### 4. Auto-Graded Tests for Immediate Feedback

For exercises and assignments, I provide automated tests:

```python
# student_solution.py
def fibonacci(n):
    # Student implements this
    pass

# tests.py (provided)
def test_fibonacci():
    assert fibonacci(0) == 0
    assert fibonacci(1) == 1
    assert fibonacci(10) == 55
    assert fibonacci(20) == 6765
```

Students run tests locally and get immediate feedback before submission.

**Why this works:**
- Instant feedback accelerates learning
- Reduces "Is my code correct?" questions
- Teaches test-driven development

### 5. Office Hours → Discord + FAQ Document

Traditional office hours don't scale. Instead:
- **Discord server** for async Q&A
- Students help each other
- I jump in for complex questions
- **Living FAQ document** with common questions/issues

**Why this works:**
- Students often answer faster than I can
- FAQ reduces repeated questions
- Available 24/7, not just office hours

### 6. Project-Based Learning with Real Problems

Instead of toy exercises, final projects solve real problems:
- Scrape real websites, build APIs
- Analyze real datasets
- Create useful tools

Recent student projects:
- Library management system with database
- COVID-19 data visualization dashboard
- Automated course schedule optimizer

**Why this works:**
- Motivation: building something real vs. "Exercise 47"
- Portfolio: students can showcase their work
- Engagement: students invest more effort

## The Numbers

Teaching at scale means tracking metrics:

**Engagement:**
- 85-90% assignment completion rate
- 60-114 stars per course repository
- Active discussions on Discord (500+ messages/semester)

**Outcomes:**
- 75-80% pass rate (Universidad Nacional is selective)
- Student projects published on GitHub
- Several students now work as Python developers

## What Doesn't Work

### ❌ Requiring Specific IDEs/Editors
Students use Windows, Mac, Linux. Some prefer VS Code, others PyCharm, some Jupyter. Fighting this is futile—teach version control and testing instead.

### ❌ Assuming Everyone Starts at the Same Level
Students range from "first time programming" to "already freelancing." Provide optional advanced challenges and foundational resources.

### ❌ Teaching Everything in Detail
You can't cover every Python feature in a semester. Focus on fundamentals (variables, functions, classes, common libraries) and teach students how to learn more.

### ❌ Grading on Style for Beginners
Early on, working code > perfect code. Introduce PEP 8 gradually. Beginners overwhelmed by "your indentation should be 4 spaces not 3" lose motivation.

## Tools That Help

- **GitHub**: Version control, assignment submission, code reviews
- **Jupyter Notebooks**: Interactive teaching, easy sharing
- **pytest**: Automated testing for assignments
- **Discord**: Async communication, peer support
- **nbgrader**: Jupyter-based grading (for quizzes)
- **Google Colab**: Zero-setup Python environment

## Advice for Teaching Python at Scale

1. **Automate what you can** (testing, grading basics)
2. **Leverage peer learning** (code reviews, Discord)
3. **Make materials reusable** (GitHub, creative commons)
4. **Teach professional practices** (version control, testing, documentation)
5. **Build community** (students helping students)
6. **Iterate annually** (improve materials based on feedback)

## Resources

Want to see the materials? Check out my GitHub:
- [GitHub Academic Profile](https://github.com/fegonzalez7)
- Course repos: pdc_unal_clase1-6, rob_unal_clase2

Teaching programming at scale? [Let's connect](/contact/) and share ideas!
