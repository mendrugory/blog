---
title: "My Experience learning Elixir with Books"
date: 2018-07-02
url: /post/my-experience-learning-elixir-with-books
---

I like to read about everything but I like too much (yes, too much) to read about tech. Nowadays we are able to find a lot of kinds of books which are a good introductory step when you are going to begin with a new technology, however most of them are only a kind of _Hello World_. Although you have read a book, you probably know nothing about the technology. And when we talk about a non-mainstream technology the problem of finding a book which covers your needs grows.

Today I can say that [Elixir](https://elixir-lang.org/) is my main language. I used to work with Python when I faced some concurrency and fault-tolerance problems that made me spend much more time on them than in the business logic. The first option was Erlang, due to my _Ericsson_ past, but Elixir caught my attention and I could not escape.

As I knew nothing I decided to find a book. [Elixir in Action](https://www.manning.com/books/elixir-in-action) was my first book. I enjoyed a lot and just after finishing I felt that I could implement some Elixir apps which would solve my problems.

During the reading of _Elixir in action_ and shaked in by the Elixir storm I booked [The Little Elixir & OTP Guidebook](https://www.manning.com/books/the-little-elixir-and-otp-guidebook) MEAP. From my point of view, the reading was even more fun and I settle my knowledge about distribution using Elixir (something that I had not used yet).

I also booked [Programming Phoenix](https://pragprog.com/book/phoenix/programming-phoenix). Phoenix is the main web-framework in Elixir and as I have said before _I use Phoenix as interface for my Elixir App_. The reading was not really enjoyable. But it is the most useful book that I have bought. You can find 90% of what you are going to need inside this short book.

After some good readings and 5 Elixir apps running in production where I have been solved my concurrency and fault-tolerance problems I decided to go a step forward and taking advantage of the distribution features of the [BEAM](https://en.wikipedia.org/wiki/BEAM_(Erlang_virtual_machine)) and I read [Designing for Scalability with Erlang/OTP](http://shop.oreilly.com/product/0636920024149.do). The book is really awesome and it helped me to improve my Erlang and also my Elixir code. The book is really pragmatic and it is based on experience what was really appreciated by me. I have to re-read this book because I felt that I can still learn more things.

Now I had my Erlang cluster which runs my real-time systems but I hade some code written in other languages with some algorithms really important. Re-writing the code was not an option because the performance was going to decrease or some developers do not develop in Elixir/Erlang and they will not. Therefore, I began to read [Erlang and OTP in Action](https://www.manning.com/books/erlang-and-otp-in-action). As always, I learnt new stuffs but specially I learnt to integrate non-BEAM code in my applications.

Engineering is a practical field it treats to solve problems in the real life, hence; **only** reading does not give you any expertise but it helps a lot as introduction, especially if you enjoy what you are reading.

PS: I was thinking about writing a book about building complex and/or event-driven systems in Elixir, maybe it is useful for someone else.