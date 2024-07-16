# Bike-Sharing-Ops-Decision-Analytics
Creating a Linear Program (LP) that coordinates bike movements for each hour of the day given an initial state and demand.

# Description of problem and LP
Our model will take the initial states and forecasted demand as inputs and output the optimal bike movements in order to maximize our profit function.

Our profit function is as follows:

$${\sf max}\;profit = \sum_{i=1}^{554}\sum_{j=1}^{554} A*(R_{ij}-O_{ij}) - M_{ij}$$

 $B_{ij}$ = Bikes Moved from Station i to Station j

We will first define two constants:
$D$ = Demand for upcoming hour

$I$ = Initial State

$D_{i} - I_{i}$: The forecasted Demand minus the Initial State of I. This can be thought of as the number of bikes missing at the station. We also want to make sure this number is greater than zero, since if the initial state is greater than the demand, there are no missing bikes, not a negative amount. Because we already know $D_{i}$ and $I_{i}$ and they are constant, we will do the calculation in advance and $max((D_{i} - I_{i}),0)$ will now be denoted as Mi.

$I_{i} - D_{i}$: The Initial state minus the forecasted Demand. This can be thought of as the number of bikes that are forecasted to be unused at the station. We also want to make sure this number is greater than zero. Once again we can do the caculation in advance and denote $max((I_{i} - D_{i}),0)$ as $U_{i}$. $U$ is the negation of $M_{i}$.


$R_{i,j}$ is the potential revenue to be gained from moving a bike from $i$ to $j$. In other words, how many bike rides are we missing out on by not having a bike there.
$R_{i,j} = min[B_{ij}, M_{j}]$ 
The amount of bike rides we can profit off of is equal to $R_{i,j}$. We use the min(), because if we move more bikes than we need, we can still only profit off of the ones we needed, and if we move less bikes than we needed, we will only profit off the bikes we moved.

$O_{i,j}$ is the opportunity cost of moving a bike away from station i. It is necessary to find this value, so that our model doesn't make the mistake of moving a bike from i to j in order to fill a need at j, without considering if the bike was needed at i.

$O_{i,j} = max[B_{ij} - U_{i}, 0]$
We use the max() to find the opportunity cost because if the number of bikes moved is less or equal to the number of unused bikes, there is no opportunity cost. If we move more bikes than there are unused, we incur an opportunity cost in bikes of $B_{ij} - U_{i}$.

It is worth repeating, $R_{i,j}$ and $O_{i,j}$ are measured in BIKES. The logical next step is to multiply the $(R_{i,j} - O_{i,j})$ by a dollar amount, A.
A = constant, avg rev per bike use.


Next, we will determine the cost of a movement.
$M_{i,j}$ is defined as the cost of one movement of bikes from i to j. Again, this is a cost per movement, NOT the cost per bike moved.
$M_{i,j} = [min (1, B_{ij})] * C$
We use a min(), so that if $B_{i,j}$ is zero, the cost is zero, and if Bij is greater than zero, there is one movement (there is no limit to bikes per truck in our problem)
C is a fixed constant price for the cost of a movement.
