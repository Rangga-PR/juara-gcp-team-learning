## Prompt Design Best Practice

Prompt engineering is designing your prompts so that the response is what you were indeed hoping to see Below are a few guidelines on how to engineer a prompts.

#### Be Concise

Do not make your prompt is unnecessarily verbose.

**Bad**: `"What do you think could be a good name for a flower shop that specializes in selling bouquets of dried flowers more than fresh flowers?"`

**Good**: `"Suggest a name for a flower shop that sells bouquets of dried flowers"`

#### Be Specific and Well-defined

Generic prompt will return a generic answer, if you are hoping to see more specific answer make your prompt as specific and well defined as possible.

**Bad**: `"Tell me about Earth"`

**Good**: `"Generate a list of ways that makes Earth unique compared to other planets"`

#### Ask one task at a time

Asking a complicated question can confuse the model. if possible breakdown your question into parts and asks it separately.

**Bad**: `"What's the best method of boiling water and why is the sky blue?"`

**Good**: `"What's the best method of boiling water?"`
**Good**: `"Why is the sky blue?"`

#### Watch out for hallucinations

Although LLMs have been trained on a large amount of data, they can generate text containing statements not grounded in truth or reality; these responses from the LLM are often referred to as "hallucinations" due to their limited memorization capabilities. Note that simply prompting the LLM to provide a citation isn't a fix to this problem, as there are instances of LLMs providing false or inaccurate citations. Dealing with hallucinations is a fundamental challenge of LLMs and an ongoing research area, so it is important to be cognizant that LLMs may seem to give you confident, correct-sounding statements that are in fact incorrect.

#### Using system instructions to guardrail the model from irrelevant responses

One way to reduce the chances of irrelevant response and hallucinations is by providing the LLM with system instructions.

```
model_travel = GenerativeModel(
    model_name="gemini-1.5-flash",
    system_instruction=[
        "Hello! You are an AI chatbot for a travel web site.",
        "Your mission is to provide helpful queries for travelers.",
        "Remember that before you answer a question, you must check to see if it complies with your mission.",
        "If not, you can say, Sorry I can't answer that question.",
    ],
)

chat = model_travel.start_chat()

prompt = "What is the best place for sightseeing in Milan, Italy?"
```

#### Turn generative tasks into classification tasks to reduce output variability

Generative tasks lead to higher output variability
The prompt below results in an open-ended response, useful for brainstorming, but response is highly variable.

`"I'm a high school student. Recommend me a programming activity to improve my skills."`

Classification tasks reduces output variability
The prompt below results in a choice and may be useful if you want the output to be easier to control.

```
"""I'm a high school student. Which of these activities do you suggest and why:
a) learn Python
b) learn JavaScript
c) learn Fortran
"""
```

#### Improve response quality by including examples

Another way to improve response quality is to add examples in your prompt. The LLM learns in-context from the examples on how to respond. Typically, one to five examples (shots) are enough to improve the quality of responses. Including too many examples can cause the model to over-fit the data and reduce the quality of responses.

##### Zero-shot Prompt

Zero-shot prompt is where you don't provide any examples to the LLM within the prompt itself.

```
"""Decide whether a Tweet's sentiment is positive, neutral, or negative.

Tweet: I loved the new YouTube video you made!
Sentiment:
"""
```

##### One-shot Prompt

One-shot prompt is where you provide one example to the LLM to give some guidance on how to response.

```
"""Decide whether a Tweet's sentiment is positive, neutral, or negative.

Tweet: I loved the new YouTube video you made!
Sentiment: positive

Tweet: That was awful. Super boring 😠
Sentiment:
"""
```

#### Few-shot Prompt

Few-shot prompt is where you provide a few examples to the LLM to give some guidance on how to response.

```
"""Decide whether a Tweet's sentiment is positive, neutral, or negative.

Tweet: I loved the new YouTube video you made!
Sentiment: positive

Tweet: That was awful. Super boring 😠
Sentiment: negative

Tweet: Something surprised me about this video - it was actually original. It was not the same old recycled stuff that I always see. Watch it - you will not regret it.
Sentiment:
"""
```

#### Choosing betweet Zero-shot, One-shot and Few-shot prompt

Choosing between zero-shot, one-shot, few-shot prompting methods
Which prompt technique to use will solely depends on your goal. The zero-shot prompts are more open-ended and can give you creative answers, while one-shot and few-shot prompts teach the model how to behave so you can get more predictable answers that are consistent with the examples provided.
