# The Local Learning Coefficient: A Singularity-Aware Complexity Measure

-  okay loss landscape exists- very complex - good solutions are not just points they can be valleys and lots of nearby weight values can be optimal - some valleys are narrow as in small change in weights can cause loss to shoot up while some are wide and more forgiving
- LLC measures how much room is there around the model'l current weight configuration
	- low llc - wide flat valley- this means there are many weights that are not really crucial to the model as in changing them wont drastically change model performance
		- high llc -  the model is dependent on all or most of its weights 
        
## singular learning theory
    learning is a sequence of phase transistions
    are there phase tansitions that can be predicted by slt?

    
    degeneracy - many solutions have the same loss, no unique global optimum
        neural networks are singular
    loss landscape geometry reflects the algorithms that are learnt (circuits)
    if we understand how data shapes the geometry, then by writing the data in a certain way we can program the learned algorithm

### foundations
    - BIC - classically used to measure model complexity and is directly proportional to number of parameters in the model
    - the nummber of good configurations tells you how much information the solution contains and we know that simpler solutions generalize better so llc telling us about simpler solutions is beneficial to us
    - what is the bayesian posterior - 
    - bayesian nn - parameters are probability distribution
    - data along with model architecture shape the loss landscape - loss landscape determines the lreaning process - learning process picks out a set of weights that set the algorithmic structure of the model
    - sgld adds kick in the form of noise for better exploration and finding optimal loss

## interpretability
    - LRH - linear representation hypothesis
        - posists that high human understandable semantic concepts are encoded as linear directions or 1 dimensional subspaces inside the activation space
    - SAE(toy models of superpositions paper) - if there is more information than there are dimensions, then information is superimposed on dimensions -  sae's take a cramped activation space and and pull i tapart into interpretable features
    - activations steering to validate tcircuits and stuff
    - this is all mainstream interp stuff




-  okay loss landscape exists- very complex - good solutions are not just points they can be valleys and lots of nearby weight values can be optimal - some valleys are narrow as in small change in weights can cause loss to shoot up while some are wide and more forgiving
- LLC measures how much room is there around the model'l current weight configuration
	- low llc - wide flat valley- this means there are many weights that are not really crucial to the model as in changing them wont drastically change model performance
		- high llc -  the model is dependent on all or most of its weights 

-     loss landscape geometry reflects the algorithms that are learnt (circuits) beacuse walgorithms/circuits decide which weights matter in the model
    if we understand how data shapes the geometry, then by writing the data in a certain way we can program the learned algorithm

- you trained 2 models and you get same final loss but which one learnt better? - how do you measure this? - simpler solutions tend to generalize better
    - simple way to measure simplicity is to count parameters, more weights more complex
- loss landscape - every point on curve is a weight configuration and height is loss - narrow pit means tiny region of weight configurations give low loss, network depends on precise settings so complex
- wide valley - lots of weight configurations give low loss - simple

- why is parameter count inaccurate measure of model complexity?
    - obviously two models witht the same number of parameters can have learned two different things, one might generalize one might memorize but loss is same in both cases
    - some parameters may not even matter to the model
    - equivalent to measuring how complicated a book is by counting its pages including like blank pages and repeated pages

 
- so only if we assume that the model is regualr, it has no redundant weights and stuff, then we can say parameter count is a good measure of complexity
- but in reality networks are singular ie diff model configurations can give the same function
- that is why we need a singularity aware complexity measure


- the more complex the model is lower is the test accuracy

- complexity is the number of bits required to describe the solution
    - for example me telling aaryan to meet me at apoorva takes fewer words than telling him to meet me at the third bench to the left from the entrance to apoorva
    - so wrt this, what they're saying is the complexity of the graph is the number of bits you'll need to specify a point inside that region.
    - the region im talking about is the region of the loss landscape where chqanges in the weight configurations does not change the loss.
    
    - B(w)* is a small neighbourhood around w*
    - B(w, ε) = { w ∈ B(w) | L(w) − L(w*) < ε } this is the good region ie all weights in the neighbourhood whose loss is within ε of the minimum. Its volume is V(ε). 
    - what the llc does for us is tells us how how V(ε) shrinks as ε → 0
    - the region we're talking about is this disk, its area is proportional to epsilon

    - in the bottom row, loss is 0 along both axes, so the minumim is 2 crossing lines - singular model
    - the good region here is the cross/star shape 
    - its volume shrinks proportional to epsilon to the 1 4th
    - therefore lambda is 1/4

    - so if epsilon gets 10,000 times smaller, the volume above also gets 10,000 times smaller but the value below only gets 10 times smaller
    - e is loss tolerance btw (how much higher than the minimujm loss you are willing to accept)
    - so larger the lambda, faster the volume shrinks
    - why do we care about volume shrink rate again again? - because fast shrinking means many different weight configurations are constrained, so the solution depends on precise weight configurations which means higher complexity... slower shrinking means means weight configurations are more realxed so low complexity

    Equation (2): the volume

    V(ε) = Vol(B(w, ε)) = ∫ dw over B(w, ε)**

    This is just the volume of the good region: all weights near w* with loss within ε of the minimum. The integral ∫dw means “add up all the tiny pieces of space inside that region”, which is how you compute volume.

    Equation (3): the bits

    bits = −log₂( V(ε) / Vol(B(w)) )*

    Vol(B(w))* is the volume of the whole neighbourhood around w*.
    V(ε) / Vol(B(w))* is the fraction of the neighbourhood that’s good.
    −log₂ of a fraction is the number of bits needed to pick out that fraction. A fraction of 1/2 takes 1 bit, 1/4 takes 2 bits, 1/1024 takes 10 bits.

    how many bits it takes to specify a good set of weights

    now as you can see this bit count is dependent on epsilon whivh is just some arbitrary number you can pick
    we need something that doesnt depend on epsilon
    

    - LLC comes in
        - so they decided to keep the term lambda as the measure of complecity and that is the local learning coeffecient and it depends on the w(weight configuartion) that we choose.
        - this is just the general formula for volume


    - how do we compute llc?
        - so we defined llc based on how we the volume scaled right, but you cant compute volumes directly for a network that has like a million weights
        - so they estimate lambda through something called free energy
        - there are two estimators that they discuss , one is idealised which is the exact formula for lambda but cant be computed in actual networks
        - i havent understood what they did here i need more time to grasp it
        - so for now ill skip to the next section

    - the next dsection is about them checking to see if their llc estimator is accurate, useful and if it works on large networks


    - so now that you have a decent enough idea of the llc i can explain what my reseacrh proposal was 
        - so its already been established that llc tracks learning changes during training with supervised learning
        - my proposal was, when i train an llm with rlvr, it gets better, but what actually changes within the model as in does it correspond to better generalisation. better generalisation measn circuit formation. 
        so ill first check to see what  happen during training by doing causal analysis
        then i test whether changes in llc show up at the same time as and when there are internal circuit changes that we find through causal analysis.
        if the answer is yes, then we can use llc as a cheap screening tool instead of circuit analysis.

        why does this matter- because right now rl trained models are judged by nechmark scores, so my project tests if a geometric measurement can tell us how the model changed during trasingin before deployment.