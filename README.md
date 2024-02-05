# Trial-CrewAI

I'm testing Crew AI by putting together a team of AI agents to work as content writers. My goal is to find and describe 10 scary places worldwide in 2023. Here's my AI agents team:

- Master Ghost Hunter
- Senior Writer content

---

# Code Here

```python
#!/usr/bin/env python
# coding: utf-8

# Install

!pip install crewai
!pip install -U duckduckgo-search
!pip install --upgrade --quiet langchain-google-genai pillow
!pip install --upgrade --quiet wikipedia

# Import

import os
from crewai import Agent, Task, Crew, Process
from langchain_google_genai import ChatGoogleGenerativeAI
# from langchain.tools import DuckDuckGoSearchRun (for duck duck search)
from langchain.tools import WikipediaQueryRun
from langchain_community.utilities import WikipediaAPIWrapper

# API

# api = Your KEY API

llm = ChatGoogleGenerativeAI(model="gemini-pro",
                             verbose=True,
                             temperature=0.6,  # ความหลากหลายของคำตอบ
                             google_api_key=api)

# search_tool = DuckDuckGoSearchRun()
search_tool = WikipediaQueryRun(api_wrapper=WikipediaAPIWrapper())

# Agent

researcher = Agent(
    role="Master Ghost Hunter",
    goal='Find the scariest places in the world and share that experiences.',
    backstory="""The seasoned Master Ghost Hunter, born into a family with a 
    mystical heritage, has explored haunted sites worldwide, from ancient European castles to 
    abandoned North American asylums and remote Asian temples. Armed with occult knowledge and advanced equipment""",
    verbose=True,  # output เพิ่มมากขึ้น
    allow_delegation=False,  # อนุญาติให้ทำงานร่วมกับ Agent ตัวอื่น
    llm=llm,
    tools=[search_tool],
)

writer = Agent(
    role='Senior Writer content',
    goal="Expertly writes stories about haunted and frightening places.",
    backstory="""Meet the mysterious blogger who passionately writes eerie tales on her blog. 
    Born with a fascination for the macabre, she draws inspiration from haunted places and folklore, 
    captivating readers with chilling stories that leave them unnerved.""",
    verbose=True,
    allow_delegation=True,
    llm=llm
)

# Task

# Create tasks for your agents
task1 = Task(
    description="""Find and tell the 10 most haunted places from around the world that you think are the scariest in 2023. 
    The story must include the name and the place as well as the story that made the place scary in detail.""",
    agent=researcher
)

task2 = Task(
    description="""Write a scary story from the information you received to make it even scarier but not distort the truth, 
    as well as make it more interesting. The final answer has to be the 10 scariest places written to attract people to read..""",
    agent=writer
)

# Crew

# Instantiate your crew with a sequential process
crew = Crew(
    agents=[researcher, writer],
    tasks=[task1, task2],
    verbose=2,  # You can set it to 1 or 2 to different logging levels
    process=Process.sequential
)

# Get your crew to work!
result = crew.kickoff()

print(result)




```
