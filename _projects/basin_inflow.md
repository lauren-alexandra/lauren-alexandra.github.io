---
layout: page
title: Basin Inflow
description: Regional Hydroclimate Deep Neural Network
img: assets/img/basin_inflow/Sacramento-River-Bend-Outstanding-Natural-Area-BLM.jpeg
importance: 1
category: work
related_publications: true
---

<p><b><a href="https://github.com/lauren-alexandra/basin-inflow-dnn" target="_blank">Basin Inflow<b></em> is an implementation of a deep neural network for forecasting reservoir inflow in the American River Basin. The proposal model is composed of stacked LSTM layers, with regularization applied to both (L2, dropout, and recurrent dropout), followed by a densely connected layer. Model application and evaluation uses river basin data with temporal coverage from 2008-2022 from the California Data Exchange Center.</p>

<p>Across the basin and along forks of the river, daily precipitation, temperature, snow water content and depth, and river discharge and stage are employed to predict local reservoir inflow. Data was pre-processed with a 30-day exponential moving average to give recent weather events more weight in the forecast, normalized, and made stationary for training. 14 years of data were separated into three sets: training set (2008-2017), validation set (2018–2020), and test set (2020-2022). The model achieves a lower MAE score (0.036) in inflow prediction than the baseline model.</p>

<div>
    <h2>Land Acknowledgment</h2>
    <p>
    The land currently occupied by the Folsom Reservoir is part of the unceded territory and traditional home of the Nisenan people. In spite of centuries of genocide and occupation, the sovereign Nisenan people persist as caretakers of the American River Basin. 

    The basin’s Nisenan Tribal entities include the Nevada City Rancheria, the Colfax-Todds Valley Consolidated Tribe, and the United Auburn Indian Community. The Nevada City Rancheria and the Colfax Rancheria were once both federally recognized, but Congress passed the California Rancheria Termination Act (1958) and later an amendment (1964) which ceased their federal recognition. Most rancherias have been restored, but Nevada City and Colfax are still trying to restore their federal status.
    </p>
    <br />
    <h4><em>Yo’ Dok’im Pakan (Gerjuoy North Fork Preserve), 2022</em></h4>
    <img src="assets/img/basin_inflow/north-fork-preserve.png" width="600" height="500" alt="gerjuoy-north-fork-preserve" />
    <p>
    <em>Note.</em> From left: Colfax-Todds Valley Consolidated Tribe chairman Clyde Prout III and Placer Land Trust executive director Jeff Darlington at Yo’ Dok’im Pakan. Placer Land Trust transferred ownership of the preserve to the tribe in 2022. From “California Ancestral Homelands Returned to Colfax-Todds Valley Consolidated Tribe,” by K. Ferguson, 2022, https://landtrustalliance.org/blog/california-ancestral-homelands-returned-to-colfax-todds-valley-consolidated-tribe. Copyright 2024 by Land Trust Alliance.
    </p>
</div>

<h4>References</h4>

<div>
Akins, D. B., & Bauer Jr., W. J. (2021). *We are the land: A history of Native California*. University of California Press. 
<br />
Arax, M. (2020). *The dreamt land: Chasing water and dust across California*. Vintage Books.
<br />
Bureau of Reclamation. (n.d.). *Folsom Dam joint federal project*. https://www.usbr.gov/mp/mpr-news/docs/factsheets/folsom-dam.pdf 
<br />
Bureau of Reclamation. (2021). *Water reliability in the West - 2021 SECURE Water Act report*. https://www.usbr.gov/climate/secure/docs/2021secure/2021SECUREReport.pdf
<br />
California Department of Fish and Wildlife. (2023, December 13). *CDFW releases beavers into the wild for first time in nearly 75 years*. https://wildlife.ca.gov/News/Archive/cdfw-releases-beavers-into-the-wild-for-first-time-in-nearly-75-years
<br />
East, A. E., & Grant, G. E. (2023). A watershed moment for western U.S. dams. *Water Resources Research, 59*(10). https://doi.org/10.1029/2023WR035646 
<br />
Huang, X., Stevenson, S., & Hall, A. D. (2020). Future warming and intensification of precipitation extremes: A “double whammy” leading to increasing flood risk in California. *Geophysical Research Letters, 47*(16), 1-9. https://doi.org/10.1029/2020GL088679 
<br />
Ingram, B. L., & Malamud-Roam, F. (2015). *The West without water: What past floods, droughts, and other climatic clues tell us about tomorrow*. University of California Press. 
<br />
Isenberg, A. C. (2006). *Mining California: An ecological history*. Hill and Wang.
<br />
Mount, J., Grenier, L., Hanak, E., Peterson, C., Bardeen, S., Cole, S., Gartrell, G., Gray, B., Morales, Z. J., & Sencan, G. (2023). *Priorities for California’s water: Stewarding the wet years*. Public Policy Institute of California. https://www.ppic.org/publication/priorities-for-californias-water
<br />
Naz, B. S., Kao, S., Ashfaq, M., Gao, H., Rastogi, D., & Gangrade, S. (2018). Effects of climate change on streamflow extremes and implications for reservoir inflow in the United States. *Journal of Hydrology, 566*, 359-370. https://doi.org/10.1016/j.jhydrol.2017.11.027 
<br />
Roseville Historical Society. (n.d.). *Inhabitants before 1820*. https://www.rosevillehistorical.org/before-1820 
<br />
Sacramento Public Library Authority. (2021). The early years. *Images of America: Lower American River* (pp. 11-38). Arcadia Publishing.
<br />
Sacramento Public Library Authority. (2021). The untamed river. *Images of America: Lower American River* (pp. 109-127). Arcadia Publishing.
<br />
Smith, J. F. (2006). *Nature noir: A park ranger’s patrol in the Sierra*. Mariner Books.
<br />
State of California. (2024). *Beaver conservation release* [Photograph]. CDFW database.
<br />
State of California. (2019, June 18). *Governor Newsom issues apology to Native Americans for state’s historical wrongdoings, establishes Truth and Healing Council.* https://www.gov.ca.gov/2019/06/18/governor-newsom-issues-apology-to-native-americans-for-states-historical-wrongdoings-establishes-truth-and-healing-council
<br />
Zamora-Reyes, D., Black, B., & Trouet, V. (2021). Enhanced winter, spring, and summer hydroclimate variability across California from 1940 to 2019. *International Journal of Climatology, 42*(9). https://doi.org/10.1002/joc.7513 
</div>
