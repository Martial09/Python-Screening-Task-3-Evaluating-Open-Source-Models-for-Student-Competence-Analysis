# RESEARCH PLAN
We will start our examination of the open source models for analyzing student competence with code-specialized Large Language Models (LLMs) because they are already designed to understand and produce both code and natural language. The model we will evaluate first is Meta's CodeLlama, specifically the 13B parameter instruct-tuned version. CodeLlama has reasonable performance and resource requirements while being explicitly instructed to follow code-related commands. We will create a curated dataset of student-written Python snippets that each contain an example of a common conceptual mistake (e.g., incorrect recursion use, misunderstanding list aliasing, mutable default arguments). Once we have the dataset in place, we will engineer a constructed meta-prompt instructing CodeLlama to evaluate each student snippet and create a Socratic question (or follow-up prompt) that probes students' understanding of the concept without revealing the underlying bug.

The process of validating will be established through two stages entailing both an automated evaluator and a human expert evaluator. In the first step, we will construct a "golden set" of ideal assessment prompts for our dataset that will be developed by an experienced Python educator, which we will then qualitatively compare against a scoring rubric to model-generated prompts based on conceptual relevance, pedagogical value, and clarity, and then establish the baselines for the golden set of prompts. For a scalable, quantitative measure, we can first evaluate and then use to evaluate the model's outputs, a powerful proprietary model like GPT-4 to score the model-generated output prompts based on our rubric. Our validation plans will effectively triangulate the assessment and provide systematic ways to evaluate CodeLlama's ability to generate useful and meaningful prompts for high-level ability analysis and the limitations like being too direct or not capturing subtle misconceptions, which will indicate if further fine-tuning is warranted.

=======================================================================================

# REASONING
1. What makes a model suitable for high-level competence analysis?                               

   A model more appropriate for analysis of higher competencies must go beyond mere syntax checking or determination of bugs. Its three main characteristics are: (1) Deep Code Understanding: The model must           understand the semantics and intent behind the code, rather than the letter of the statements being created. For example, the model will understand algorithms, design structures, and common logical pitfalls.      (2) Abstract Reasoning: The model must infer the student’s mental model and lack of understanding from their code. For instance, it should recognize that nested loops for a task that could be solved more          effectively through a dictionary lookup is indicative of the student’s understanding of data structures and time complexity. (3) Pedagogical Generative Skill: The model must have sufficient control to produce     open-ended, Socratic questions to encourage the student's thinking processes, as opposed to stating facts or corrections.
 
2. How would you test whether a model generates meaningful prompts?
    
   Testing for "meaningful" prompts necessitates going beyond simple accuracy metrics. The best way to do this is with a rubric-based human evaluation. I would develop a rubric with measures like: "Identifies the    main misconception (Yes/No," "Does not give a direct answer (Scale 1-5)," "Stimulates critical thinking (Scale 1-5)," and "Relevant to student code (Scale 1-5)." Experienced educators could then utilize this      rubric to score the model's prompts against several different code examples written by students. This qualitative data is incredibly valuable. I would follow with a comparison of the models output to prompts      developed from a human expert and a state-of-the-art proprietary model to observe its performance as a benchmark.
   
3. What trade-offs might exist between accuracy, interpretability, and cost?

   There are significant trade-offs:
   i. Accuracy vs. Cost – Bigger, more accurate models (e.g., 70B parameter models) allow for more nuanced and context-sensitive prompts, but they have substantial computational costs (more expensive GPUs, more         energy) and higher latency. Smaller, less accurate models (e.g., 7B) are cheaper to use and provide faster speeds, which may be critical in a real-time educational tool, but would more likely generate more        generic or less insightful prompts altogether.
  ii. Accuracy vs. Interpretability – Modern LLMs are essentially black boxes, and as the models become larger and more accurate, their internal reasoning becomes more opaque. We cannot easily backtrack on the          process by which it generated a specific prompt, nor is it without risk to be in an educational context where there could be a subtly flawed prompt that misleads a student. We can only ascertain the               validity of the output, not the process.
 iii. Cost vs. Interpretability – This is less direct but generally the simpler, cheaper models appear to be more predictable in modes of failure, and as such can be more manageable and can allow for a defense of       guardrails - though neither of those would be described as truly interpretable.
 
4. Why did you choose the model you evaluated, and what are its strengths or limitations?

   I selected Meta's CodeLlama as the primary model to assess.
   A. Reasons for selection:
      1. Specialization: This model is specifically pre-trained and fine-tuned on a huge body of code, thus it has a native advantage in both parsing programming languages' syntax and idioms as well as logic over           general-purpose LLMs.
      2. Open Source: This model is open source and freely available for research and commercial use, meeting the core requirement of the task.
      3. Size: This model is available in multiple size options (e.g.--7B, 13B, 34B), ideal for investigating the accuracy vs. cost trade-off directly.
   B.Strengths:
      1. Strong understanding of code syntax and semantics across multiple languages, especially Python.
      2. The "instruct-tuned" versions are specifically configured for producing solutions that provide more complex natural-language instructions, on which the meta-prompting techniques are dependent.
   C.Limitations:
      1. It has been trained specifically as a coding assistant, not as a tutor. The default behavior of the models would lean toward providing solutions or providing fixes to code directly. The implications of           this would require sophisticated prompt engineering, or even potentially further fine-tuning on a dataset developed specifically for Socratic educational dialogue and tutoring.
      2. Similar to all LLMs, it is capable of "hallucinating" or producing implausible but plausible information, which would require a validation mechanism before displaying to a student.
         Like all LLMs, it can "hallucinate" or generate plausible but incorrect information, which requires a layer of validation before its output is shown to a student.
   --------------------------------------------------------------------------------------------------------------------------------------------------------------------------
