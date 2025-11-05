# Output vs. competence: reflections on abstraction, AI, and embodiment

## Background

I interviewed for a search role. Since I work on search stuff everyday at [paralegal.lk](https://www.paralegal.lk), I assumed my background technical knowledge is up to scratch. It was also a busy work week and I did not have much time to prepare. I also carried some rust as this was my first technical interview in over two years.

Most questions at the interview focused on search fundamentals: preprocessing, indexing, basic retrieval algorithms, etc. Stuff I know quite well. Or, stuff I thought I knew quite well. As it turns out, I was totally unable to recall answers to some of these foundational questions.

Specifically, while I could visualise in each critical step up to indexing, I totally blanked on what happens from the query side. On the documents side, you start with a bunch of documents and their document ids. You lower case the text, strip away punctuation, delete stop words, stem, and then create a positional inverted index. I could also in my head picture the logical layout of the positional inverted index.

But this is where my mental model of a search engine just . . . blanked. Beyond the fact that you apply the same preprocessing steps to queries, I had nothing. The query-side mechanics had completely evaporated from my memory. I could not recall how we fetched documents. I remembered what term frequency (TF) and inverse document frequencey (IDF) are as concepts. I vaguely remembered that idf is a log function but its smoothed form we typically implement in retrieval engines did not come to my mind.


Now, the humiliating bit. My CV linked to a repo from January 2023: a Django-based case law retrieval system I'd built for a Colombo law professor. That system used BM25, handled Boolean queries (term A AND NOT term B), phrase queries ("term C term D term E") with distance tolerance, and other custom retrieval functions. During the online interview, I had the code open in front of me via screen share with my interviewers. I still couldn't recall the basic mechanics of BM25 or how it addresses the limitations of TF-IDF scoring.

## Coda

I am astonished at how much of my "education" I appear to have forgotten. This is altogether more sobering because a not so small part of my identity is built on a reputation for not forgetting. Is such forgetfulness simply a fact of aging? I (only?) turned thirty-three last month. At the risk of sounding dramatic, is there much to be done besides raging against the dying of the light?

I have reflected on my failure to recall technical details on basic search questions quite a lot since the interview and I have a degree of confidence in pronouncing that 'no, it isn't primarily a case of aging'.

Also, while the constraints of the interview format -- the pressure to reply instantly, the absence of access to our primary recall mechanism in 2025, ChatGPT, and the inability to consult reference materials -- certainly didn't help me, they are merely proximate causes. My subpar performance at the interview is primarily a function of the peculiarities of our present-day working processes. In particular, there are three elements to it.

### Rising abstraction

Although I work in search, I don't work on it. Paralegal.lk is built on the open-source search engine Typesense. In the two years of building on Typesense, I have never peered into the stored index structure, never audited which stop words get stripped from queries, never traced through the relevance scoring calculations that determine which cases surface first.

There are two reasons for this:

(1) Typesense, by design, conceals every detail that makes or breaks a search engine to deliver excellent out-of-the-box performance. But this convenience comes at a cost. I have not needed to think about the underlying search mechanics: instead, I have been a content Typesense API monkey.

(2) The real problem at paralegal.lk isn't search mechanics—it's data. You can't retrieve what you haven't indexed. I've just finished indexing 1978–2021 (Sri Lanka Law Reports), but earlier 20th-century decisions (New Law Reports) remain scattered and incomplete. Most of my time goes to hunting down cases, not tuning algorithms.

While the second cause is purely circumstantial, the first cause raises questions about the effect of abstraction on our everyday work.

In Grady Booch’s words, the story of software is one of rising abstraction. You see it most clearly in the evalution of programming languages. Hardly anyone writes Assembly. Even C programmers rarely think in terms of registers, flags, or instruction timing. Compilers and runtimes decide register allocation, instruction selection, inlining, and vectorization. In the 1980s, every software engineer would have had to worry about memory management. The vast majority no longer does so. The same pattern repeats across the stack:

- UI: HTML/CSS/JS → component frameworks (React, Vue, Svelte)
- Hosting/Compute: bare metal → cloud VMs → serverless
- Build/Deploy: custom bash scripts → push-to-deploy (Heroku/Render) → auto-deploy with previews (Vercel/Netlify)

Now, this very march of abstraction is why software runs the world: it frees up our cognition for composition and enables API monkeys like me to write and run software that does't break. Yet it also means that many of us are out of touch with the low-level machinery of how software actually works.

Is there’s a point on the curve of abstraction where we stop doing our jobs as engineers? Questions once rooted in technology are now questions of economics, and in most production systems, we simply choose between "managed" services. Is a "search engineer" who simply picks between Typesense Cloud vs. Algolia for building an e-commerce site, primarily based on projected monthly cost, really doing search engineering?

### {} AI

AI didn't directly cause me to fail the interview. But over the past two years, code assistants have quietly restructured my cognitive habits. On reflection, at least some of my struggles trace back to this shift.

The interview crystallized something I have been increasingly aware of. The more I "progressing" in my carereer, the less I access the cognitive machinery that builds real understanding and deep recall. This isn't AI's fault alone. Working in a narrow ML domain where I already know the patterns, I can autopilot through entire weeks without straining my brain. There's no demand to marshal my full cognition when the path forward is reasonably obvious.

My strength is big-picture thinking. And many skilled and observant senior engineers, to whom I’ve reported, have told me this. Details, however, have always required effort. Yet here’s the irony: to truly understand big pictures — at least the kind that’s actually worth knowing, the kind that blows your mind, the kind that's built on mathematical insight or years of experimental results — I’ve always had to work through the details. Symbolic reasoning only emerged from manipulating raw numerics, working through toy problems, and tracing patterns across such examples. Slowly, line by line, on paper. Lectures felt like wasted time. Real learning, to me, always meant sitting with a textbook, a few sheets of paper, a pen, and a calculator, and grinding through the particulars. When it came to understanding large software systems, the analogous activity of grappling with the details was stepping through the debugger and inspecting intermediate values.

Early in my programming career, writing every line forced that same focus on details. Auto-complete eroded it slightly. AI code assistants have nearly eliminated it. Now I write requirements, sketch high-level instructions, specify critical edge cases, then watch my inanimate "agents" thrash out the code. Vibe coding in known domains requires virtually no thinking. Before production, I just test thoroughly—tests that, again, the same trigger-happy agents write for me.

So AI has also had a similar effect as abstraction, of distancing me from the details that matter, details that give rise to higher levels of reasoning, and ground big picture thinking. And the resulting atrophy of critical cognitive muscles — wasting away by way of non-engagement with details — has had a debilitating effect on my general intellectual sharpness. The interview exposed this viscerally. Rust has set in what was once a well-oiled machine. Even on questions for which I answered correctly, I felt that thought was a step behind intention: answers came to me slower than I would have liked.

### Losing embodied practice

The interview reminded me how knowledge slips when I don’t work it by hand. To execute paralegal.lk's hybrid mode queries, Typesnse fuses custom text-match scores with vector similarity using reciprocal rank fusion (RRF). I’ve known this since the beginning, but I never had to revisit the math for reasons I set out under the ill effects of abstraction. Three days before the interview I skimmed a ChatGPT explainer on RRF and it all felt clear. But, for the interview question on RRF, I could recall the denominator of the RRF formula but not the numerator, which is the part that actually makes RRF tick.

My earlier quip on learning with paper, pen, and a calculator was a callback to my university days. As an electrical engineering undergraduate, in exams, I could only answer questions I had worked through by hand. Many modules ran on gnarly equations; writing them out was how they stuck. There was a suspect quality to formulae or sample problems I merely read.

Now, I almost never write on paper. My handwriting is both slower and more incomprehensible than ever.

There’s a parallel in code linked to my discussion on AI. Typing every line, as a junior engineer, burned Python’s syntax and idioms into memory. These days I write more JavaScript but totally lean on AI assistants: I would be lost without them.

Distance from the page and the keyboard is distance from the very essence of craft. My RRF stumble was just a symptom of a deeper discontent.

## The Way Forward

A couple hours of focused preparation would have spared me considerable pain. But my purpose here is not to offer tips on acing interviews. Rather, I want to redesign how I work so that I can (i) keep learning on the job, (ii) retain what I learn, and (iii) stay intellectually fit in the age of APIs and AI.

### Fighting abstraction: Touching the parts that make or break what we do

We can't avoid abstraction. But we can be conscious about what we abstract and how much we abstract.

I don't really care that I don't know React well. Building frontends is not something I see myself doing—it doesn't interest me beyond its immediate utility. But I love search, and the core of paralegal.lk is search. So I should not offload too much of search if I want paralegal.lk to teach me about search.

Here's a good heuristic for knowing what level of abstraction is appropriate: are we actively touching the parts of the code that make or break what we care about? In search, that means the stuff that matters to final search relevance—the stuff that determines whether search takes milliseconds or minutes. We must be getting our hands dirty on those parts. This is how we fight abstraction.

### Fighting AI: Keeping our fingers in the code

There's no going back to the good old days of Stack Overflow. There is no turning off AI. But we can choose how we use it.

DHH articulates this beautifully:

> Now, I actually love collaborating with AI too. I love chiseling my code, and the way I use AI is in a separate window. I don't let it drive my code. I've tried that. I've tried the Cursors and the Windsurfs and I don't enjoy that way of writing. One of the reasons I don't enjoy that way of writing is, I can literally feel competence draining out of my fingers. That level of immediacy with the material disappears.

He continues with a revealing anecdote about learning Bash:

> Where I felt this the most was, I did this remix of Ubuntu called Omakub when I switched to Linux. It's all written in Bash. I'd never written any serious amount of code in Bash before, so I was using AI to collaborate, to write a bunch of Bash with me, because I needed all this. I knew what I wanted, I could express it in Ruby, but I thought it was an interesting challenge to filter it through Bash... But what I found myself doing was asking AI for the same way of expressing a conditional, for example, in Bash over and over again. That by not typing it, I wasn't learning it. I was using it, I was getting the expression I wanted, but I wasn't learning it. I got a little scared.

This resonates deeply. Is this the end of learning? Am I no longer learning if I'm not typing?

DHH's solution: don't give up on AI—it's such a better experience as a programmer to look up APIs, to get a second opinion on something, to do a draft—but do the typing yourself. You learn with your fingers. If you're learning how to play the guitar, you can watch as many YouTube videos as you want, you're not going to learn the guitar. You have to put your fingers on the strings to actually learn the motions. There is a parallel here to programming, where programming has to be learned in part by the actual typing.

But the more crucial aspect: staying competent. As DHH notes, even great programmers lose their edge the moment they put away the keyboard. This happened even before AI, as soon as people got promoted.

> If you don't have your fingers in the sauce (the source) you are going to lose touch with it. There's just no other way. I don't want that because I enjoy it too much. … The joy of a programmer … is to type the code myself.

So the lesson is not to be output driven. When I optimise for output, as I have been over the past two years, I will always choose AI. I am going to reorient myself towards competence. I took a small step on that journey.

### Fighting disembodiment: Back to paper and pen

Last Saturday, I took the train back from Jaffna to Colombo. Before leaving, I dusted up the *Introduction to Information Retrieval* textbook (physical) by Manning et al. after four years. My scientific calculator had run out of battery; I got a new Casio ES-991 Plus from Poobalsingham Bookstore. During the journey, I hand computed BM25 scores for a query against a toy corpus I created. I turned it into a repo: https://github.com/elihoole/nano-bm25. After a slow start, I zoned in. I tasted again the joys of deep focus. The deliberate pace. The tactile satisfaction of pen on paper. The joy of the math on paper matching with the math on code.

This was embodied learning. And I need more of it.

