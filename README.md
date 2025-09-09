# MultiLogicNMRer
This is the github repository of the paper "MultiLogicNMR(er): A Benchmark and Neural-Symbolic Framework for Non-monotonic Reasoning with Multiple Extensions" published in EMNLP2025

## 1. Non-nomotonic Reasoning (NMR) Dataset Description

The Datasets directory contains three sub-datasets (MultiLogicNMR, MultiLogicNMR_OOD, MultiLoigcNMR_NL), each of which contains datasets in credulous reasoning mode and skeptical reasoning mode. Each sample contains three questions. 

**MultiLogicNMR** is a multi-extension NMR dataset generated based on a template. The number of extensions in each sample of MultiLogicNMR belongs to R={1, 2, 3, 4, 5}

**MultiLogicNMR_OOD** is a NMR dataset generated based on a template for evaluating the generalization ability of the model in terms of the number of extensions. The number of extensions in each sample of MultiLogicNMR_OOD belongs to R={6, 8, 10, 12, 16 } 

**MultiLogicNMR_NL** is a multi-extension NMR dataset rewritten by a large language model. MultiLogicNMR_NL is aimed for evaluating the robustness of models in the diversity of natural languages.

### A data sample format is as follows:

{

% The Number of Sample.

**Sample_number**: 1 

% A set of facts expressed in a formal language

**Origin_Facts**: 

          talkative(morgan). love(morgan,orson). energetic(morgan). -drab(morgan). -persistent(morgan). faithful(morgan). mock(morgan,orson). -laugh(morgan,orson). fat(morgan). -busy(morgan). -average(morgan). 

% The number of facts included in the sample

**Facts_number**: 11

% A set of rules expressed in a formal language (default rules)

**Defalut_Rules**: 

          -octagonal(X):-talkative(X),-drab(X), not -combative(X), not educational(X).strong(X):--busy(X),-persistent(X), not -hollow(X), not graceful(X).-educational(X):-fat(X),strong(X), not -poor(X).-poor(X):--octagonal(X), not -educational(X).-hollow(X):--octagonal(X), not strong(X), not beautiful(X).-combative(X):--amused(X), not -educational(X), not gleaming(X).significant(X):-faithful(X),energetic(X), not -poor(X).-old(X):--laugh(X,Y), not sudden(X).-amused(X):-love(X,Y),-average(X), not -crowded(X).aggressive(X):-mock(X,Y),-old(X), not -educational(X).

% A collection of factual sentences represented in natural language

**NL_Origin_Facts**: 

          Morgan is talkative. Morgan love Orson. Morgan is energetic. Morgan is not drab. Morgan is not persistent. Morgan is faithful. Morgan mock Orson. Morgan does not laugh Orson. Morgan is fat. Morgan is not busy.Morgan is not average.

% A set of rules for natural language representation

**NL_Defalut_Rules**: 

          If someoneA is talkative and not drab then he is not octagonal,unless he is not combative or he is educational.If someoneA is not busy and not persistent then he is strong,unless he is not hollow or he is graceful.If someoneA is fat and strong then he is not educational ,unless he is not poor.If someoneA is not octagonal then he is not poor ,unless he is not educational.If someoneA is not octagonal then he is not hollow,unless he is strong or he is beautiful.If someoneA is not amused then he is not combative,unless he is not educational or he is gleaming.If someoneA is faithful and energetic then he is significant ,unless he is not poor.If someoneA does not laugh someoneB then he is not old ,unless he is sudden.If someoneA love someoneB and someoneA is not average then he is not amused ,unless he is not crowded.If someoneA mock someoneB and someoneA is not old then he is aggressive ,unless he is not educational. 

% The number of extensions included in this sample

**Origin_ASP_extension_number**: 1 

% Questions with formal language representation

**Origin_Question_Text_Lists**:

          [-octagonal(morgan). -strong(morgan). -poor(morgan).] 

% Questions with natural language representation

"**NL_Origin_Question_Text**": ["Morgan is not octagonal.", "Morgan is not strong.", "Morgan is not poor."], 

% The Label of the questions

**Origin_Question_Label_Lists**: [["T"], ["F"], ["M"]], 

}

## 2. Code Description

The Code file contains some sub-files. 

**Models_on_MultiLogicNMR**：include the code of MultiLogicNMRer with different LLM on MultiLogicNMR datasets. For example, the MultiLogicNMR_Solver_on_credulous_by_DeepSeek.py file in the Models_on_MultiLogicNMR refers to the code of MultiLogicNMRer based on DeepSeek on the MultiLogicNMR dataset in credulous reasoning.

**Models_on_MultiLogicNMR_OOD**: include the code of MultiLogicNMRer with different LLM on MultiLogicNMR_OOD datasets.

**Models_on_MultiLogicNMR_NL**: include the code of MultiLogicNMRer with different LLM on MultiLogicNMR_NL.

**Models_on_LogicNMR**: include the code of MultiLogicNMRer with GPT4o-mini on LogicNMR.
