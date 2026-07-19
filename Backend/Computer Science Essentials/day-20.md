# Day 20: Networking Fundamentals

**Week 3 · Advanced Algorithms & Systems**

## 🎯 Learning Objectives
- Explain what happens between typing a URL and seeing a webpage
- Understand the TCP/IP model and where HTTP fits
- Reason about latency, DNS, and connection setup costs

## 📚 Topics
- The TCP/IP model (link, internet, transport, application)
- TCP vs UDP
- DNS resolution
- TLS handshake & HTTPS at a high level
- Latency vs bandwidth

## 💡 Core Ideas
- TCP trades some speed for reliability (ordered, guaranteed delivery); UDP trades reliability for speed — the right choice depends entirely on whether occasional loss is acceptable.
- DNS resolution, TCP handshake, and TLS handshake all add round trips *before* your actual request even starts — this is why connection reuse (keep-alive, HTTP/2) matters so much for performance.
- Latency (time per round trip) and bandwidth (data per second) are different bottlenecks — a high-bandwidth, high-latency connection still feels slow for chatty protocols.

## 🛠️ Practice Exercises
- [ ] Write out, step by step, everything that happens (DNS, TCP, TLS, HTTP request/response) when you type a URL into a browser and press enter.
- [ ] Use a terminal tool (`curl -v`, `dig`, or your OS equivalent) to observe DNS resolution and the TLS handshake for a real site.
- [ ] Explain in your own words why HTTP/2 multiplexing reduces the impact of latency compared to HTTP/1.1.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why does TCP guarantee delivery and ordering while UDP doesn't, and what's the cost of that guarantee?
- What's the purpose of the TLS handshake, and why does it add latency to the first request?
- How does DNS caching improve subsequent request latency?

## 🔗 Resources
- https://howhttps.works/
- https://www.smashingmagazine.com/2021/06/network-request-life-cycle/

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[⬅ Previous Day](day-19.md) | [🏠 Home](README.md) | [Next Day ➡](day-21.md)
