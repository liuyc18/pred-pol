# Literature Survey
> LI LINGYU, LIU YICHAO, WU JINGYAN, YANG QINGSHAN, YE FANGDA (Group 5)

## Question Definition
- **Predictive Policing** is defined as *"the application of analytical techniques particularly quantitative techniques – to identify likely targets for police intervention and prevent crime or solve past crimes by making statistical predictions".* It can be categorized into 4 main classes: 1. Predicting Places of Increased Crime Risk, 2. Predicting Potential Offenders, 3. Predicting Group/Population Crime Patterns and 4. Predicting Potential Victims.      |

## Summary of Existing Systems and Their Effectiveness
- **PredPol**, introduced around 2012, became most commonly implemented in the United States. It focuses on place-based or “hotspot” policing, using primarily three types of historical data: crime type, location, and time. By analyzing these data, it generates risk maps highlighting areas with a high probability of crime. 

Two core mathematical ideas behind PredPol are the reaction-diffusion model and the Epidemic-Type Aftershock Sequence (ETAS) model. These models capture the notion that crimes tend to cluster and repeat in nearby locations and times (so-called “near-repeat” or “contagion” effects).
    - **reaction-diffusion model**: describes the spread of crime in a city as a chemical reaction. The model assumes that crime spreads from one location to another in a similar way to how chemicals diffuse in a liquid.
    - **ETAS model**: treat the dynamic occurence of crime as a continuous time, discrete space epidemic-type aftershock sequence point process. The model is based on the idea that crimes are not isolated events but are influenced by past crimes in the same area.
    - Effectiveness: Early assessments of PredPol in cities like Santa Cruz, California, documented an approximately 19% decrease in burglaries within the designated zones. In Los Angeles, pilot programs similarly reported notable reductions in property crime, suggesting that the model can guide law enforcement toward more efficient resource allocation. These findings point to the potential utility of predictive policing when applied under controlled conditions.
    - Drawbacks: Certain examinations of PredPol’s deployment like 2019 in Oakland revealed that reliance on historical crime data can reinforce existing disparities. Researchers found that the algorithm repeatedly flagged the same neighborhoods, resulting in heightened police scrutiny of marginalized communities. Such patterns raise ethical concerns regarding bias and over-policing.

- **Other Systems and Approaches** 
    - HunchLab: A more complex predictive policing platform that integrates crime data, geographic points/lines/polygons, and temporal data like weather or school schedules. It also incorporates demographic information, aiming to forecast the likelihood of specific crime types in particular places and times.
    - Crime Anticipation System (CAS)(Netherlands): Operates on the principle that crimes are not randomly distributed but are concentrated in certain geographic and temporal clusters. The system integrates geographic clustering methods with temporal crime trends, recognizing that recent crimes often predict future crimes in nearby areas within a short time window. 
    - Risk Terrain Modelling(RTM): emphasizes environmental factors like abandoned buildings or liquor stores, and uses machine learning to incorporate sociological data.
    - People-based or Group-based Policing: attempts to identify individuals or groups at higher risk of committing or being victims of violence, such as gun violence. However, these methods face ethical concerns regarding profiling and data accuracy.

## Review of the Modelling Approaches
> PredPol uses a straightforward statistical approach assuming that crime event follows a statistical distribution with two parameters: space and time. It believes past crimes are indicative of future crimes trends in nearby areas.
#### 1. reaction-diffusion model
In a discrete time model, taken *broken window effect* into consideration, we have
\[\begin{equation}
    B_s(t + \delta t) = \left[ B_s(t) + \frac{\eta \ell^2}{z} \Delta B_s(t) \right] (1 - \omega \delta t) + \theta E_s(t)
\end{equation}\]
where $B_s(t)$ is a dynamic value models the risk of a site being attacked at time $t$. $\omega$ sets a time scale over which repeat victimizations are most likely to occur, and $\theta$ is a multiplier of $E_s(t)$, which is the number of burglary events that occurred at site $s$ since time $t$. $0 \leq \eta \leq 1$ measures the significance of neighbor effect. $l$ is the size of grid, $z$ is the number of sites $s^{'}$neighoring $s$ .$\Delta$ is the discrete Laplacian, whereby
\[\begin{equation}
    \Delta B_s(t) = \frac{1}{\ell^2} \left( \sum_{s' \sim s} B_{s'}(t) - z B_s(t) \right)
\end{equation}\]
From the discrete model, we form the difference quotient:

\[\begin{equation}
\frac{B_s(t + \delta t) - B(t)}{\delta t}
\end{equation}\]

and take the limit as $\ell$ and $\delta t$ approach 0 to arrive at the differential equation

\[\begin{equation}
\frac{\partial B}{\partial t} = \frac{\eta D}{z} \nabla^2 B - \omega B + \epsilon D \rho A.
\end{equation}\]

Here we have denoted

\[\begin{equation}
D = \frac{\ell^2}{\delta t}, \quad \epsilon = \theta \delta t, \quad \rho(s, t) = \frac{n_s(t)}{\ell^2},
\end{equation}\]

and $\rho$ is the density of criminal agents

\[\begin{equation}
\frac{\partial \rho}{\partial t} = \frac{D}{z} \nabla \cdot \left[ \nabla \rho - \frac{2\rho}{A} \nabla A \right] - \rho A + \gamma,
\end{equation}\]

where offenders exit the system at the rate $\rho A$ and are reintroduced at the constant rate $\gamma = \Gamma / \ell^2$. 

The PDE for $\rho$ is obtained by a difference quotient for $ n_s(t) $, using the equation

\[\begin{equation}
n_s(t + \delta t) = A_s \sum_{s' \sim s} \frac{n_{s'}(t)(1 - p_{s'}(t))}{T_{s'}(t)} + \Gamma \delta t.
\end{equation}\]

where 
\[\begin{equation}
    T_{s^{'}} = \sum_{s^{''} \sim s^{'}} A_{s^{''}}(t)
\end{equation}\]
which simply means that any agents that are present at $s$ after one time step must have either arrived from a neighboring site after having not committed a crime there, or have been generated at $s$ at rate $\Gamma$. 

#### 2. Using EM algorithm in ETAS model
ETAS model assumes a continuous time, discrete space(square grids) problem. It supposes that each event generates $N \sim Possion(\theta)$ direct offspring events.
In this model, policing areas are discretized into square boxes. The probabilistic rate of events in box $n$ at time $t$ is defined to be

\[\begin{equation}
\lambda_n(t) = \mu_n + \sum_{t_n^i < t} \theta \omega e^{-\omega (t - t_n^i)},
\end{equation}\]

where $t_n^i$ are the times of events in box $n$ in the history of the process. The background rate $\mu$ is a (nonparametric histogram) estimate of a stationary Poisson process.

The expectation, or E-step, sets

\[\begin{equation}
p_n^{ij} = \frac{\theta \omega e^{-\omega (t_n^i - t_n^j)}}{\lambda_n(t_n^j)}, \quad p_n^j = \frac{\mu_n}{\lambda_n(t_n^j)},
\end{equation}\]

where $\theta \omega e^{-\omega t}$ is called **the triggering kernel** that models "near-repeat" or "contagion" effects in crime data.

The maximization, or M-step, sets

\[\begin{equation}
\omega = \frac{\sum_n \sum_{i < j} p_n^{ij}}{\sum_n \sum_{i < j} p_n^{ij} (t_n^j - t_n^i)},
\end{equation}\]

\[\begin{equation}
\theta = \frac{\sum_n \sum_{i < j} p_n^{ij}}{\sum_n \sum_j 1},
\end{equation}\]

\[\begin{equation}
\mu = \frac{\sum_n \sum_j p_n^j}{T},
\end{equation}\]

where $T$ is the length of the time window of observation.
A more detailed derivation can be find [here](https://github.com/arun-ramamurthy/pred-pol/blob/master/doc/Rederivation%20of%20Mohler%20et%20al.pdf).

## Feature Engineering Techniques
    - Temporal Features: Day-of-week, time-of-day, seasonality flags like holidays
    - Spatial Features: Clusters, proximity measures, geohashes, distance to police stations, environmental factors like bars/hotspots
    - Crime Type Encoding: One-hot encoding, classify by severity, further classify to high/low risk of arresting
    - Aggregated Rates: Crime counts per area/time window, arrest counts per area/time window, repeat domestic incident flags

## Evaluation Metrics and Their Appropriateness
PredPol and other systems employ a range of quantitative and qualitative methods to assess its predictive policing system.
-   Prediction Success Rate: gauges the proportion of actual crimes that occur within the areas designated as crime hot spots. For example, a 2015 study by PredPol claimed that its model accurately predicted 4.7% of crimes and asserted it was twice as accurate as traditional methods.
-   Changes in Crime Rate: Based on historical record, the Santa Cruz Police Department observed an 11% reduction in residential burglaries and a 27% reduction in robberies after deploying PredPol.
-   False Positive/Negative Rates: Simulation studies suggest that feedback loops may cause the system to over-predict crime in certain neighborhoods. For example, an analysis of drug-related crime forecasts in Oakland found that communities of color were misidentified at roughly twice the rate of predominantly white areas.

However, these metrics face serious problems due to potential data bias, lack of algorithmic transparency, and broader concerns about social justice.
-   Lack of Transparency: Limited transparency exacerbates concerns about policing inequities, particularly among marginalized groups that have historically experienced disproportionate surveillance. 
-   Oversimplification of Social Complexities: By focusing primarily on spatial and temporal factors, predictive tools may overlook deeper social drivers of crime, such as poverty and educational inequality.
-   Short-Term Gains vs. Long-Term Consequences: Even when short-term crime rates decline, community trust can suffer, potentially heightening tensions between law enforcement and the public in the long run.