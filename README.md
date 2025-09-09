# MultiLogicNMRer
This is the official repository for the paper "MultiLogicNMR(er): A Benchmark and Neural-Symbolic Framework for Non-monotonic Reasoning with Multiple Extensions" publish by EMNLP 2025.

This repository contains the implementation of the MultiLogicNMRer framework and the three related datasets introduced in the paper. Non-monotonic reasoning (NMR) refers to the fact that conclusions may be invalidated by new information, a key aspect of human reasoning. An NMR task often results in multiple plausible sets of conclusions, known as "extensions". This project provides tools to evaluate and enhance the ability of Large Language Models (LLMs) to handle such complex reasoning scenarios.

![Figure 1: An example of multi-extension NMR](https://github.com/sysulic/MultiLogicNMRer/blob/main/multiextension_example.png)

(Note: This figure provides an intuitive example of how a diagnosis for a patient with chest pain changes from "acid reflux" to potentially "acid reflux" or a "heart attack" after learning about "shortness of breath".)


# Datasets
The Datasets directory contains three benchmark datasets (MultiLogicNMR, MultiLogicNMR_OOD, MultiLoigcNMR_NL) designed to evaluate Non-monotonic Reasoning. Each dataset includes both skeptical and credulous reasoning modes. Every sample contains a context with facts and default rules, along with three questions. The datasets are generated via a comprehensive pipeline that includes: 1) generating formal default theories, 2) solving them with an ASP (Answer Set Programming) solver to find all extensions, 3) translating the formal logic into natural language templates, and 4) rewriting the templated text with an LLM to increase linguistic diversity.

![Figure 1: An example of multi-extension NMR](https://github.com/sysulic/MultiLogicNMRer/blob/main/Dataset_Generate_Framework.png)

(Note: This figure illustrates the four-stage dataset generation process.)


**MultiLogicNMR** This is the core multi-extension NMR dataset.

1. Purpose: To evaluate multi-extension NMR capabilities of LLMs.

2. Number of Extensions: Each sample has a number of extensions belonging to the set {1, 2, 3, 4, 5}.


**MultiLogicNMR_OOD** This dataset was created to assess the generalization ability of models to a larger number of extensions.

1. Purpose: To evaluate out-of-distribution generalization based on the number of extensions.

2. Number of Extensions: Each sample has a number of extensions belonging to the set {6, 8, 10, 12, 16}.

**MultiLogicNMR_NL** This dataset was constructed by rewriting samples from MultiLogicNMR with an LLM (GPT-4o-mini) to increase linguistic variety. To ensure logical correctness, the rewriting process was validated using semantic equivalence and predicate alignment checks.

1. Purpose: To evaluate the robustness of models against diverse natural language expressions.


2. Number of Extensions: Same as MultiLogicNMR, {1, 2, 3, 4, 5}.


### Data Format

Each data sample is a JSON object with the following fields:

{

% A unique identifier for the sample.

**Sample_number**: 1 

% A set of facts expressed in a formal language

**Origin_Facts**: 

          talkative(morgan). love(morgan,orson). energetic(morgan). -drab(morgan). -persistent(morgan). faithful(morgan). mock(morgan,orson). -laugh(morgan,orson). fat(morgan). -busy(morgan). -average(morgan). 

% The total number of facts in the sample.

**Facts_number**: 11

% A set of rules expressed in a formal language (default rules)

**Defalut_Rules**: 

          -octagonal(X):-talkative(X),-drab(X), not -combative(X), not educational(X).strong(X):--busy(X),-persistent(X), not -hollow(X), not graceful(X).-educational(X):-fat(X),strong(X), not -poor(X).-poor(X):--octagonal(X), not -educational(X).-hollow(X):--octagonal(X), not strong(X), not beautiful(X).-combative(X):--amused(X), not -educational(X), not gleaming(X).significant(X):-faithful(X),energetic(X), not -poor(X).-old(X):--laugh(X,Y), not sudden(X).-amused(X):-love(X,Y),-average(X), not -crowded(X).aggressive(X):-mock(X,Y),-old(X), not -educational(X).

% The natural language representation of the facts.

**NL_Origin_Facts**: 

          Morgan is talkative. Morgan love Orson. Morgan is energetic. Morgan is not drab. Morgan is not persistent. Morgan is faithful. Morgan mock Orson. Morgan does not laugh Orson. Morgan is fat. Morgan is not busy.Morgan is not average.

% The natural language representation of the default rules.

**NL_Defalut_Rules**: 

          If someoneA is talkative and not drab then he is not octagonal,unless he is not combative or he is educational.If someoneA is not busy and not persistent then he is strong,unless he is not hollow or he is graceful.If someoneA is fat and strong then he is not educational ,unless he is not poor.If someoneA is not octagonal then he is not poor ,unless he is not educational.If someoneA is not octagonal then he is not hollow,unless he is strong or he is beautiful.If someoneA is not amused then he is not combative,unless he is not educational or he is gleaming.If someoneA is faithful and energetic then he is significant ,unless he is not poor.If someoneA does not laugh someoneB then he is not old ,unless he is sudden.If someoneA love someoneB and someoneA is not average then he is not amused ,unless he is not crowded.If someoneA mock someoneB and someoneA is not old then he is aggressive ,unless he is not educational. 

% The number of plausible conclusion sets (extensions) for the sample.

**Origin_ASP_extension_number**: 1 

% Questions with formal language representation

**Origin_Question_Text_Lists**:

          [-octagonal(morgan). -strong(morgan). -poor(morgan).] 

% Questions with natural language representation

"**NL_Origin_Question_Text**": ["Morgan is not octagonal.", "Morgan is not strong.", "Morgan is not poor."], 

% The ground-truth labels for the questions (T: True, F: False, M: Unknown).

**Origin_Question_Label_Lists**: [["T"], ["F"], ["M"]], 

}

# Code

The Code directory contains the implementation of the MultiLogicNMRer neural-symbolic framework. This framework is based on the bound-based algorithm for ASP solving and uses LLMs to perform single-step reasoning operations.

 ![Figure 1: An example of multi-extension NMR](https://github.com/sysulic/MultiLogicNMRer/blob/main/Framework.png)
 
 (Note: This figure shows the six modules of the MultiLogicNMRer framework: Grounding, Bound Initialization, Reduction, Reasoning, Selection, and Label Generation.)


**Models_on_MultiLogicNMR**：Contains the code for running MultiLogicNMRer with different LLMs on the MultiLogicNMR dataset.

For example, the MultiLogicNMR_Solver_on_credulous_by_DeepSeek.py file in the Models_on_MultiLogicNMR refers to the code of MultiLogicNMRer based on DeepSeek on the MultiLogicNMR dataset in credulous reasoning.

**Models_on_MultiLogicNMR_OOD**: Contains the code for running MultiLogicNMRer with different LLMs on the MultiLogicNMR_OOD dataset.

**Models_on_MultiLogicNMR_NL**: Contains the code for running MultiLogicNMRer with different LLMs on the MultiLogicNMR_NL dataset.

**Models_on_LogicNMR**: Contains the code for running MultiLogicNMRer with GPT-4o-mini on the LogicNMR dataset, as described in the experiments.

For example, the file MultiLogicNMR_Solver_on_credulous_by_DeepSeek.py in the Models_on_MultiLogicNMR directory refers to the code for the MultiLogicNMRer framework using the DeepSeek model on the MultiLogicNMR dataset in credulous reasoning mode.
