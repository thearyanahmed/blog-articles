Probably, we’ve seen a meme/quote where it says, 
> Software development is like being a detective in a crime scene where you are also the criminal.   

I’ve worked here for some time. Around 2.3 years. Not that much. But I still have connections to many of the top tier companies. I’m writing regarding some of the problems I have faced and my ex-colleagues face. 

### Don’t Write Tests
Writing tests are seen a waste of time, in many companies. Where I’ve been working with a greek company for almost 1 year (2021) and for the last 2-3 months, I’ve been actively looking a new job.

During this time, while I had a job or not, I had to write tests. Not for my personal projects. But even when I applied for interviews, one part of the process was to complete an assignment. 
And by that time, it was like if I don’t write tests, it’s same as not submitting the assignment at all.

Let’s talk about why we should write tests, though it’s gonna be redundant for many.

#### Why we should write tests
I’m not going into too much details, but here are some simple points.

1. ~Avoid manual testing~
Manual testing is time consuming, and also fault tolerant towards human errors. I’ve seen situations where the “Human tester” gives the thumbs up and in production it blows up 🤯.   
2. ~Confidence~
Be confident what you have created. Even if something doesn’t work, you’ll know its not gonna work. And knowing what’s not gonna work is way better than not knowing what’s gonna work and what not.

Also you’ll know what works. 😃
4. ~Your code will change~
Your code will change over time. Even if you working in the best possible way, following scrum/agile or something else and the requirement is clear from the client/product owner and down the line, you’ll see that your feature changes. 

One example could be, once we were building a multi-vendor multi-currency based e-commerce. At the beginning we discussed and got confirmation that we’ll have a base currency and every other currency will be converted. 

After implementing the feature, the whole thing changed. In one line. 
But was it that easy for us to change the whole thing? It had 4 mobile apps and client + merchant + admin panels. 

4. ~Easier Bug Fixes~
Well, run your tests and you get a good number red flags. Your tests will work in some sort of interfaces, not OOP interfaces but a bridge between your user/consumer and your code.

5. ~Self Documentation~
Your test will be a documentation of its own. A new developer, someone from a different team or a new recruit can read the tests and get a glimpse of how the application should behave.

There are many other benefits. But, 
### Writing tests is expensive
Writing tests costs resources. Yes. But this is not necessarily a valid point to skip tests. In any mid-long term application, that grows with time, not writing tests is like taking a leap of faith where you are mostly probably gonna jump into a pit. 
And it can also be a bottomless pit. Where you’ll have so much dependency that decoupling them would be a nightmare and your management will be telling you continuously to push new features, why doesn’t this work? When will that work?  

### In this part of the world
But one thing I don’t understand is that who can Europeans and Americans write tests and we don’t find time? I know not everyone write tests. 

But take a look at the ratio, the difference is noticeable. We have successfully built a culture of not writing tests and that is somehow a good thing. 
But they don’t see that not writing test will only cost them more in the not-so-long term. 

Almost every course you do; almost every book you read regarding software development, almost every conference you join, none of them will tell you to write tests.  It is a tradition. 

I wrote almost because I’ve never read/heard/been in a book/conference/course that you should write tests. 

### What’s it like not writing tests

![](https://raw.githubusercontent.com/thearyanahmed/blog-articles/master/images/what_it_like_not_writing_tests.jpg)

