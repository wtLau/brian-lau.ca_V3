---
title: 'Learning Rust'
publishDate: '2023-05-23'
excerpt: "Brian's thought process in the shift to Rust for frontend development, highlighting its benefits in control, safety, reliability, and performance."
isFeatured: true
tags:
  - Rust
seo:
  image:
    src: '/post-7.jpg'
    alt: Bright lines on a dark background
---

![Bright lines on a dark background](/post-7.jpg)

## Introduction and Background

I am not a Rust developer yet, but within a year or two, I plan to shift my focus to Rust for all my software development projects. Over the past few years, I have worked with various software technologies, including robotics operating systems, Linux Ubuntu, NeoVim, and web systems. One common theme I've observed is that software systems can be overwhelming and complex, with only a few individuals truly understanding the overall architecture. Assumptions made by those unfamiliar with the system can lead to anomalies over time, resulting in technical debt, migrations, and reduced efficiency. Simply throwing more resources at the problem, such as increasing computational power, doesn't solve the underlying issues.

## The Limits of Computation Resources

Computation resources are finite, and there comes a point where adding more hardware no longer scales. Previously, I assumed that modern systems would scale effortlessly with advanced distribution systems and good engineering practice. However, as we push the boundaries of computer science, there are physical limitations that require tremendous effort to overcome, potentially taking decades or centuries. Nevertheless, it is worth exploring how we can write better instructions to minimize waste. In the past, only a gifted group of coders who understood low-level programming language details could accomplish this, but today, open-source concepts and abundant online resources make it more accessible.

## How Rust Enhances Frontend Development

As a frontend developer, TypeScript is my primary tool. While its dynamic nature and fast iteration speed can be advantageous, it can also introduce complexity if not managed intentionally. The need for improved control, safety, reliability, and performance has become more apparent. Rust, with its ability to pierce through abstractions, offers a creative environment for designing great software. The bridge between Rust and the frontend is WebAssembly (WASM), a binary format that runs in web browsers. WASM is already supported in various web development environments, and although it may not replace major workflows, it is an attractive tool for handling frontend use cases such as large data processing, concurrent data streams, cross-platform logic, and security-sensitive vulnerabilities. In the long run, I believe Rust will handle computation in large-scale frontend codebases, while JavaScript/TypeScript will maintain their role in interactivity with HTML and CSS.

## Conclusion and Learning Rust as a Frontend Developer

In conclusion, my plan is to transition to Rust for my software development projects. I see Rust as a tool that offers better control, safety, reliability, and performance in frontend development through WebAssembly. I anticipate that Rust will become the standard for managing logic in large-scale frontend codebases, while JavaScript/TypeScript will continue to handle interactivity. Although learning Rust as a frontend developer presents challenges, it is certainly achievable with dedication and practice. Cheers! 🦀
