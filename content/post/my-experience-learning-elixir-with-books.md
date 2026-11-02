---
title: "My Experience learning Elixir with Books"
date: 2018-07-02
url: /post/my-experience-learning-elixir-with-books
---

I like reading about everything, but I like reading about tech too much (yes, too much). Nowadays we can find many kinds of books that are a good first step when you are starting with a new technology; however, most of them are just a kind of _Hello World_. Even after reading one, you probably still know nothing about the technology. And when we talk about a non-mainstream technology, the problem of finding a book that covers your needs gets bigger.

Today I can say that [Elixir](https://elixir-lang.org/) is my main language. I was working with Python when I faced some concurrency and fault-tolerance problems that made me spend much more time on them than on the business logic. My first option was Erlang, due to my _Ericsson_ past, but Elixir caught my attention and I could not escape.

As I knew nothing, I decided to find a book. [Elixir in Action](https://www.manning.com/books/elixir-in-action) was my first one. I enjoyed it a lot, and right after finishing it I felt I could build some Elixir apps that would solve my problems.

While reading _Elixir in Action_, and swept up by the Elixir storm, I pre-ordered the MEAP of [The Little Elixir & OTP Guidebook](https://www.manning.com/books/the-little-elixir-and-otp-guidebook). In my opinion it was even more fun to read, and it consolidated my knowledge of distribution with Elixir (something I had not used yet).

I also pre-ordered [Programming Phoenix](https://pragprog.com/book/phoenix/programming-phoenix). Phoenix is the main web framework in Elixir and, as I have said before, _I use Phoenix as the interface for my Elixir app_. It was not a very enjoyable read, but it is the most useful book I have bought. You can find 90% of what you are going to need in this short book.

After some good reads and 5 Elixir apps running in production, where I had solved my concurrency and fault-tolerance problems, I decided to go a step further and take advantage of the distribution features of the [BEAM](https://en.wikipedia.org/wiki/BEAM_(Erlang_virtual_machine)), so I read [Designing for Scalability with Erlang/OTP](http://shop.oreilly.com/product/0636920024149.do). The book is really awesome and it helped me improve both my Erlang and my Elixir code. It is very pragmatic and based on experience, which I really appreciated. I have to re-read it, because I feel I can still learn more from it.

Now I had my Erlang cluster running my real-time systems, but I had some code written in other languages with some really important algorithms. Rewriting that code was not an option, because performance would decrease, or because some developers do not write Elixir/Erlang and never will. So I began reading [Erlang and OTP in Action](https://www.manning.com/books/erlang-and-otp-in-action). As always, I learnt new things, but above all I learnt how to integrate non-BEAM code into my applications.

Engineering is a practical field that tries to solve real-life problems, so reading **alone** does not give you any expertise, but it helps a lot as an introduction, especially if you enjoy what you are reading.

PS: I have been thinking about writing a book on building complex and/or event-driven systems in Elixir; maybe it would be useful for someone.