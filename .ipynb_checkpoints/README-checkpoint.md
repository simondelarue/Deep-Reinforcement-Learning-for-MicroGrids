<p align="center">
<img src="img/global_picture.png" width="600" height="250" />
</p>

# Reinforcement learning approach for MicroGrid energy supply

## Team and project overview 

**Bréboin Alexandre, Delarue Simon, Nourry Mathias, Pannier Valentin**

<details open="open">
<summary><h3 style="display: inline-block">Team presentation</h2></summary>
We are four student pursuing a Post Master's degree in Big Data at Télécom Paris, all coming to this Hackathon with different background :<br>  
    - <b>Alexandre</b> : "Engineer with a Managing and Consulting background, I'm am delighted to work on my technical skills with this Hi!ckathon !"<br> 
    - <b>Simon</b> : "Engineer with 5 years experience in banking sector and AI enthusiast, I'm pleased to join this Hi!ckaton !"<br>  
    - <b>Mathias</b> : "Engineer with 3 years experience in energy sector and management of IT solutions in indsutrial field, this Hi!ckathon was a great way to apply my knowledge !" <br>
    - <b>Valentin</b> : "Holding a Master's degree in Physics, I'm moving to the Data domain especially dealing with Deep learning approaches." <br>
<br>    
To tackle this interesting challenge, we first focused on understanding the context of energy supply when dealing with microgrids and did exploratory data analysis to get a good intuition about the different metrics.  
Then, as our team contains several data scientists, we chose to investigate both the "rules based" approach and the "simple" Reinforcement approach. Indeed, it allowed us to compare the results at first round, then to improve our learning agent (RL approach) with the context knowledge we gathered. Finally, as the challenge get to an end, we developped a <b>Deep Q-Learning</b> approach which indeed lowered our final budgets on buildings.
</details>

<details open="open">
<summary><h3 style="display: inline-block">General strategy</h2></summary>
We developped an algorithm able to learn by itself an improved strategy for energy supply management, for 3 different buildings. This algorithm found an efficient strategy to use energy, in terms of environmental aspect - as we focused on frugality - as well as financial aspect, since one of its objective is reducing final energy cost.
</details>

## Scientific approach

<details open="open">
<summary><h3 style="display: inline-block">Approach description</h2></summary>
    
After a quick look at the data, we used the "rules-based" naïve approach proposed in the hackaton to get a <b>baseline model</b>. The purpose of this model is to have a quick intuition about the possible results.<br>
    
Then, to solve the probem raised in the hackaton, we've decided to work in parrallel on two sides of the subject :<br>
- The <b>understanding of data</b>. We focused on developping human-based rules to manage the energy supply, which were guidelines to have in mind when developping and testing autonomous agent<br>
- The implementation of a <b>reinforcement learning</b> based approach, in which we gave an agent : <br>
    - some basic rules to manage supply <br>
    - some <b>crafted rules</b> to try and find by itself the best strategy to minimize final cost for energy<br>
    - <b>penalization</b> at Q-table initialization, in order to drive our agent's choices to maximize specific comportements (battery use for example) <br>
    
Since the results of this approach were outperforming baseline, yet still quite low regarding the "theoretical" values given in the project, we developped a third approach, based on <b>Deep Q-Network</b>. In this method, we only gave our agent the most simplest rules and used neural network power to improve its long term strategy.

<p align="center">
<img src="img/RL.png" width="450" height="250" />
</p>

<h3 style="display: inline-block">"Simple" Reinforcement learning</h3></summary>

<b>Crafted-rules</b> <br>

- We reduced the total price by trying and remove a maximum of the <b>loss-load</b> of our system<br>
- We focused on reducing the <b>pv_curtailed</b> of our system in order to maximize the use of already available ressources<br>
- We focused on the agent's ability to charge/use its <b>battery</b> when needed<br>
    
<b>Q-table penalization</b> <br>
    
The buildings 1 & 2 Q-tables were initialized not completely randomly, but rather with penalization if <b>battery energy</b> is not sufficiently used. Indeed, we saw that the agent had a particuliar approach on this use and was often trying to import energy from the grid, rather than using its own capacities.<br>
    
    
As the 3d building was specific in its energy management possibilities (genset available, yet with a bad ecological footprint, and power cuts), we decided to initialize the <b>Q-table</b> (state/action cross-product) <b>not completely randomly</b>, but rather with <b>penalization</b> on the genset actions. <br>
This drived the choices of our agent, in the direction of a better ecological impact on the long term, by reducing its reward policy when he chose to use the genset. Beyond the ecological part of the problem, this method showed slightly better cost after a year of choices by our agent.
    
<h3 style="display: inline-block">Deep Reinforcement learning</h3></summary>
    
<b>Architecture</b> <br>
    
We chose an architecture with respect to the <b>frugality/performance trade-off</b> :
- <b>Layers : </b> 1 hidden layer, 1 input layer, 1 output layer, all with 64 units<br>
- <b>Activation function : </b> ReLU <br>
- <b>Batch size : </b> 64 <br>
- <b>Optimize : </b> Adam <br>

<h3 style="display: inline-block">Results</h3></summary>

<h4 style="display: inline-block">1. Baseline</h4></summary>
   
The <b>baseline approach</b> results on test buildings are as following : 
    
- Building 1 : <b>14 399.5€</b><br>
- Building 2 : <b>48 012.3€</b><br>
- Building 3 : <b>43 901.5€</b><br>
    
<h4 style="display: inline-block">2. "Simple" Reinforcement learning</h4></summary>

In this method, we tried to find the best trade-off between frugality (time of training agent and cost generated by CPU) and performance. Thus, we trained our models on 15 episodes for all 3 buildings.<br>

The <b>"simple" reinfocement learning</b> approach results on the test buildings are as following : 
    
- Building 1 : <b>8 728.2€</b><br>
- Building 2 : <b>29 951.0€</b><br>
- Building 3 : <b>37 758.1€</b><br>
    
<b>Frugality</b> <br>
    
- Training : <b>350.6s</b><br>
- Test : <b>21.8s</b><br>
    
<h4 style="display: inline-block">3. Deep Reinforcement learning</h4></summary>

This approach gave us the best results on our problem by far.
Finally, the <b>Deep reinfocement learning</b> approach results on the test buildings are as following (in parenthesis, relative error with theoretical values): 
    
- Building 1 : <b>4 119.9€ </b>(+12.3%)<br>
- Building 2 : <b>13 749.1€ </b>(+12.4%)<br>
- Building 3 : <b>16 429.0€ </b>(+19.9%)<br>
    
<b>Frugality</b> <br>
    
- Training : <b>6696s</b><br>
- Test : <b>5s</b><br>
    
</details>

<details open="open">
<summary><h3 style="display: inline-block">Future improvements</h2></summary>
    During this project, we had troubles training our agent with the <b>poor state dictionnary</b>. Indeed, its low range of values did not reflect some interesting aspects of the problem, like the price or the level of curtailement. An incomplete answer to this is the method we developped ; adding crafted rules to our actions. But, to go further, we could use a Machine Learning model to predict actions based on features newly created to retrieve these information.<br>
    
Another improvement would be to train our agent on different windows of time. Indeed, at the moment, it learns on windows of <b>1 hour</b>, but we could hypothetize that similar patterns in energy demand (and thus supply need) would emerge only on <b>day-long</b> windows (night and days) or even <b>week-long</b> windows (working day vs week ends). This could potentially gives our agent more intuition about the needs in energy at specific moments, and then lead it to learn more complex strategies in term of price/efficiency optimization.
</details>


## Project usage

**Files**

*  data/ : Directory containing _pickle_ files for building 1, 2 and 3
*  results/ : Directory containing _json_ files for results (Deep RL) on building 1, 2 and 3
* `Requirements.txt` : Packages requirements
* `DiscreteEnvironment.py` : Contains the original `DiscreteEnvironment`class
* `DiscreteEnvironment_modified.py` : Contains the `DiscreteEnvironment`class, with **crafted** rules
* `Submission_notebook.ipynb` : Notebook containing code for "Simple" and Deep **reinforcement learning** implementations. Agents are training and testing on all 3 buildings.

**Usage**

Clone this repository and run the `Submission_notebook.ipynb` notebook.
