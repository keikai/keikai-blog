---
layout: post
title:  "Keikai 7 vs Keikai 6: Benchmark Results"
date:   2026-07-26
categories: "Application"
excerpt: "Keikai 7 replaces the Apache POI-based backend with a native Rust engine. Here are the measured gains for opening, saving, and working with large workbooks."

index-intro: "
Keikai 7 replaces the Apache POI-based backend with a native Rust engine. Here's what that means for opening files, saving files, and bulk cell operations.
"
image: "2026-07-keikai7-benchmarks/cover.svg"
tags: developer performance benchmark keikai7
author: "Jean Yen"
authorImg: "/images/author/jean.jpg"
authorDesc: "Marketing, Keikai."

imgDir: "2026-07-keikai7-benchmarks"
javadoc: "http://keikai.io/javadoc/latest/"
---

![]({{ site.baseurl }}/images/{{page.imgDir}}/cover.svg)

# Keikai 7 vs Keikai 6: Benchmark Results

Keikai 7 replaces the Apache POI-based backend with a native Rust engine. This post reports the measured impact on the scenarios that matter most in real applications.

---

## Introduction

Keikai 7 replaces the Apache POI-based backend with a native Rust engine. This post reports the measured impact on the scenarios that matter most in real applications: opening files, saving files, bulk cell operations, and loading large workbooks in the browser.

Keikai 7 is currently in beta, and the numbers below are from the current eval build.

## What changed under the hood

Keikai 6 used Apache POI internally to read, write, and evaluate Excel workbooks. Keikai 7 replaces this with a native Rust engine, shipped as a JNI library. File I/O, formula evaluation, and bulk cell access no longer go through POI's Java data structures and code paths.

For most applications, the upgrade is a version bump. Applications that customize on top of Apache POI directly may need adjustments — details in the upgrade guide.

## The numbers

### File I/O

| Scenario | Keikai 6 | Keikai 7 | Speed-up |
|---|---:|---:|---:|
| Open a real 18.7 MB Excel workbook (cached) | 16.5 s | 2.4 s | **~7× faster** |
| Save a large 10.9 MB workbook | 7.5 s | 2.4 s | **~3× faster** |
| Save a formula-dense 100k-formula sheet | 0.41 s | 0.12 s | **~3.4× faster** |

### Bulk cell operations

| Scenario | Keikai 6 | Keikai 7 | Speed-up |
|---|---:|---:|---:|
| Set 1,000,000 cells (batch) | 2.98 s | 0.47 s | **~6× faster** |
| Read 1,000,000 cells (batch) | 249 ms | 105 ms | **~2.4× faster** |

### Opening large workbooks (browser-visible)

| Scenario | Keikai 6 | Keikai 7 | Speed-up |
|---|---:|---:|---:|
| Open a 1M-formula workbook | 14.5 s | 5.5 s | **up to ~2.6× faster** |
| Open a 100k-formula workbook | 1.58 s | 0.82 s | **~1.9× faster** |
| Open a 1M-cell value workbook | 2.15 s | 1.38 s | **~1.5× faster** |

## Why these scenarios in particular

The numbers above are consistent with what the architectural change would predict:

- **File open** is faster because the native engine trusts the cached values that Excel writes into every `.xlsx` file, and skips unnecessary recomputation on load.
- **File save** is faster because the native serializer writes `.xlsx` directly, without going through POI's `SXSSFWorkbook` layer.
- **Bulk cell operations** are faster because per-cell overhead drops significantly when cells are held in native memory rather than as individual Java objects.

None of these are new optimizations bolted on to the previous engine — they are direct consequences of replacing the underlying implementation.

## How we measured

Both versions run against byte-identical benchmark code, fixtures, and driver scripts. The only difference between a Keikai 6 run and a Keikai 7 run is which Keikai jars are on the classpath (Keikai 6 uses Apache POI; Keikai 7 uses the native Rust engine).

Each measurement is 10 timed runs per version in ABAB blocks, with a fresh JVM per version and JDK 17. Server-side tests (file open, save) were measured on an idle machine and show tight spreads, so the exact multipliers are reliable. Browser-visible tests (bulk operations, large workbook loading) were measured under real machine load, so those numbers are reported as approximate (~) multipliers rather than exact figures.

## What this means

For applications that spend meaningful time opening, saving, or working with large workbooks, Keikai 7 removes a substantial portion of that cost. The gains are largest where the previous engine spent the most time in POI's data structures — which happens to be exactly where enterprise workloads tend to hit performance limits.

Keikai 7 is in beta now. You can try it in the [Keikai 7 live demo](#) or read the [Keikai 7 beta release notes](#) for details.
