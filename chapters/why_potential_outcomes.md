
The context
- We – that is, you, me, and a few colleagues you'll meet along the way – work at an e-commerce company.
- We have learned (from prior data, user experience testing, customer feedback, etc.) that our users find it difficult to evaluate whether a product is right for them.
- Our team thinks that adding feature A to the PDP might alleviate the problem.
- So, we want to answer the question "does feature A help our customers better evaluate whether a product is right for them?". 
- To answer that question, we need to make it more precise:
	- How do we measure "ease of product evaluation"? Say we know that metric $Y$ is a good proxy for it.
- ... (formulate hypothesis...)




Why potential outcomes?
- Clarifies what precisely we are trying to estimate: average individual treatment effect
- Makes explicit the assumptions we need to make to do so: assuming that Y0 are the same in expectation (sth else?)
 Next:
 - Answer above question, and clearly write down outline of entire story including assumptions (I have that, just revisit and make sure it's complete) - have bre assuming n known as basis (which is basically CRE). For now that's fine.
	 - Assumptions needed
		 - SUTVA
			 - What we need is to be able to write Y-i = WY1 + (1-W)Y0. For this we need i) independence from other's assignment, and ii) clearly defined meaning of Wi =1 and Wi = 0, becauese if they are not clearly defined then Y1/Y0 might not be stable. SUTVA handles both of these.
		- Randomisation: to make sure that EY0 for W=1 equals 
		- EY0 W=0
 - Then read cunningham2021causal (chapter 4) and titiunik2020natural for neat setup and write my section based on this
 - Write up entire thing (i.e. unbiasedness and derivation of std)
 - Then get feedback from colleagues and determine whether it's worth doing BRE without assumption of fixed n, and whether it's worth doing superpopulation perspective.
 - At the same time, keep going with further sections: power, estimation, inference, ...

- [ ] With replacement / IID approach
	- [ ] Same as BRE?
	- [ ] Relation to above approaches
	- [ ] Why do we need potential outcomes?
- [ ] Without replacement
	- [ ] Same as CRE?
- [ ] Use [[stats_of_online_experiments]] to add context

Material
- ding2023first footnote 2 in chapter 4 and 




### Notes


# 



## 
