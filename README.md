# NBA MVP Forecasting

<img src="https://github.com/user-attachments/assets/b3cbcc2a-783e-4380-b56f-92ebc0482e9c" width="600"/>

## Description
Code: [nba-mvp-forecasting.ipynb](https://github.com/PiotrLehmann/NBA-MVP-Forecasting/blob/a7a7164f88e1649cb52a3a364613d31966c157d1/nba-mvp-forecasting.ipynb)
</br>
</br>
This project focuses on applying machine learning methods to predict the results of the **NBA Most Valuable Player (MVP)** voting. The main goal was to develop a tool that, based on the statistics of all players in a given season, forecasts the ranking of the top 5 players as well as the predicted MVP winner. Value which model is predicting is called **MVP Votes Share**. The MVP Votes Share indicates the proportion of votes received by a player out of all possible MVP votes in a given season. It is calculated using the formula:

**MVS = PV / TGV**
where:  
- PV — number of votes received by the player  
- TGV — total number of votes cast in the voting

This metric was used because the number of voting experts varies each year, making the vote count alone a less reliable target variable. Players who received no votes were assigned a value of 0.0.

## Model
Due to the strong imbalance in the dataset with respect to the MVP Votes Share, the Random Forest model was chosen for its robustness to such imbalance. Additionally, this model does not require normalization or standardization of the data, which simplified the interpretation and evaluation of the results.

## Dataset
Available under [Kaggle - Clear Dataset](https://www.kaggle.com/datasets/piotrlehmannml/clear-dataset/data) contains NBA player statistics from the 2001–2023 seasons, prepared for training and testing machine learning models. Data was collected and merged from four types of tables available on Basketball Reference: per-game stats, advanced stats, cumulative stats, and team standings. Whole data collection and preparation process was fully done by me. The final test dataset (containing only stats form 2024-2025 season) is available [here](https://www.kaggle.com/datasets/piotrlehmannml/final-nba-tester/data). Note that is was downloaded 06.12.2024.

Key preparation steps:
- Collected data from 23 seasons, focusing on recent years to capture up-to-date patterns,
- Unified and merged data for each season,
- Handled traded players by consolidating their stats under the one team code
- Standardized column names and removed unnecessary columns,
- Added a `seed` column representing the team’s rank in the season standings and `mvp_votes_share` as the target variable — the share of MVP votes received by each player,
- Split data into training (~80% of seasons) and testing (~20%), with test seasons chosen to represent various historical contexts,
- Filled missing values with the column means to not loose data and not confuse the model,
- Applied threshold filtering on the training set by removing players with zero MVP vote share who did not meet minimum statistical criteria,

## My Own Predictor
A custom evaluation metric was developed for the model based on RBO - Ranked Biased Overlap (easily explained [here](https://changyaochen.github.io/Comparing-two-ranked-lists/)), which measures similarity between rankings with emphasis on top positions. This metric compares the predicted ranking of the top 5 players with the actual ranking for each season. It evaluates how well the model orders players rather than precisely predicting their MVP Votes Share values, which are affected by data imbalance. Using RBO significantly improved the model’s ability to forecast the MVP winner. Finally, this metric was used to create a predictor that evaluates the model on a scale from 0 to 1, where 1 represents a perfect match of the REAL season’s ranking.

## Results
As a result of training various models and identifying the most effective one, forecasts for the NBA Most Valuable Player (MVP) were generated for each season since the year 2000 [➡️ To this file](documents/best_model_prediction_results.pdf), where green color means right prediction / red means bad prediction. In addition, the remaining four MVP contenders were also predicted in order to later compare the results with predictions made by the NBA organization itself.
Results reached:
- ✅ **86.9%** accuracy in prediction of MVP only,
- ✅ **81.7%** in prediction of top 5 favourites for MVP race

Additionally, after performing the final prediction for the 2024–2025 season (on December 6, 2024, while the season was still ongoing), the model's effectiveness was confirmed by selecting the same MVP favorite as the one chosen by the NBA experts or official NBA model — as shown in the image below.

<img src="https://github.com/PiotrLehmann/NBA-MVP-Forecasting/blob/35dd01a23786031c138f18ab868d24f66bc6cab2/documents/final_prediction.png" width="1000">
