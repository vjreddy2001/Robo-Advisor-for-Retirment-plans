# Robo-Advisor-for-Retirment-plans
# The Power of the Cloud and Unsupervised Learning

![robo_advicer](https://user-images.githubusercontent.com/83671629/129432649-6f937763-6282-4380-a1ef-5bd22bb0f32d.jpg)


## Background

It is time to take unsupervised learning and the AWS services and apply it to new situations. 

As a digital transformation consultant for one of the most prominent retirement plan providers in the country, I want to help the company increase their client portfolio, especially by engaging young people. Since machine learning and NLP are disrupting finance to improve customer experience, I decided to create a robo advisor that could be used by customers or potential new customers to get investment portfolio recommendations for retirement.

In this Tool, I combined my new Amazon Web Services skills with already mastered Python superpowers, to create a bot that will recommend an investment portfolio for a retirement plan.

The following main tasks were accomplished:

1. **[Initial Robo Advisor Configuration:](#Initial-Robo-Advisor-Configuration)** Define an Amazon Lex bot with a single intent that establishes a conversation about the requirements to suggest an investment portfolio for retirement.

2. **[Build and Test the Robo Advisor](#Build-and-Test-the-Robo-Advisor):** Made sure that the bot is working and responding accurately along with the conversation with the user, by building and testing it.

3. **[Enhance the Robo Advisor with an Amazon Lambda Function:](#Enhance-the-Robo-Advisor-with-an-Amazon-Lambda-Function)** Created an Amazon Lambda function that validates the user's input and returns the investment portfolio recommendation. This task includes testing the Amazon Lambda function and making the integration with the bot.

---

### Files

* [lambda_function.py](RoboAdvisor.py)
* [correct_dialog.txt](correct_dialog.txt)
* [age_error.txt](age_error.txt)
* [incorrect_amount_error.txt](incorrect_amount_error.txt)
* [negative_age_error.txt](negative_age_error.txt)

---

### Instructions

#### Initial Robo Advisor Configuration

Here I have created the `RoboAdvisor` bot and add an intent with its corresponding slots.

Signed into the AWS Management Console and [create a new custom Amazon Lex bot](https://console.aws.amazon.com/lex/home). 
Used the following parameters:

* **Bot name:** RoboAdvisor
* **Output voice**: Salli
* **Session timeout:** 5 minutes
* **Sentiment analysis:** No
* **COPPA**: No
* **Advanced options**: No
* *Left default values for all other options.*

Created the `RecommendPortfolio` intent, and configure some sample utterances as follows:

* I want to save money for my retirement
* I'm ???`{age}???` and I would like to invest for my retirement
* I'm `???{age}???` and I want to invest for my retirement
* I want the best option to invest for my retirement
* I'm worried about my retirement
* I want to invest for my retirement
* I would like to invest for my retirement

This bot will use four slots, three using built-in types and one custom slot named `riskLevel`. 
I Defined the three initial slots as follows:


| Name             | Slot Type            | Prompt                                                                    |
| ---------------- | -------------------- | ------------------------------------------------------------------------- |
| firstName        | AMAZON.US_FIRST_NAME | Thank you for trusting me to help, could you please give me your name? |
| age              | AMAZON.NUMBER        | How old are you?                                                          |
| investmentAmount | AMAZON.NUMBER        | How much do you want to invest?                                           |

The `riskLevel` custom slot will be used to retrieve the risk level the user is willing to take on the investment portfolio. 
Created this custom slot as follows:

* Select the `+` icon next to 'Slot Types' in the 'Editor' on the left side of the screen.
* Choose `create custom slot` from the resulting display window.
* For **Slot type name**, type: riskLevel
* Select the radial dial button next to **Restrict to Slot values and synonyms**, then filled in the appropriate values and synonums. *Example*: Low, Minimal; High, Maximum.
* Click `Add slot to intent` when finished.

To format the response cards for the intent, click on the gear icon next to the intent as seen in the image below:

![gear_icon](https://user-images.githubusercontent.com/83671629/129461642-44ce2500-1790-4810-99c8-55e6b0778476.png)
s/gear_icon.png)

Next, input the following data in the resulting display window:

* **Prompt:** What level of investment risk would you like to take?
* **Maximum number of retries:** 2
* **Prompt response cards:** 4

Configure the response cards for the `riskLevel` slot as is shown bellow:

| Card 1                              | Card 2                              |
| ----------------------------------- | ----------------------------------- |
| <img src="card1.png" width="250" height="250">  | <img src="card2.png" width="250" height="250">

| Card 3                              | Card 4                              |
| ----------------------------------- | ----------------------------------- |
| <img src="card3.png" width="250" height="250">  | <img src="card4.png" width="250" height="250">  |

**Note:** You can find free icons from [this website](https://www.iconfinder.com/) 

Move to the *Confirmation Prompt* section, and set the following messages:

* **Confirm:** Would you like me to search for the best investment portfolio for you now?
* **Cancel:** I will be pleased to assist you in the future.

Leave the error handling configuration for the `RecommendPortfolio` bot with the default values.

![Error handling configuration]<img width="1084" alt="error_handling" src="https://user-images.githubusercontent.com/83671629/129461659-0f90f5b5-192c-4104-a230-dc6ddf952d28.png">


#### Build and Test the Robo Advisor

Here I have tested the Robo Advisor. To build the bot, click on the `Build` button in the upper right hand corner. Once the build is complete, test it in the chatbot window. 
You should see a conversation like the one below.

![Robo Advisor test](RobAdvisor_video.gif)

#### Enhance the Robo Advisor with an Amazon Lambda Function

In this section, you will create an Amazon Lambda function that will validate the data provided by the user on the Robo Advisor. Start by creating a new lambda function from scratch and name it `recommendPortfolio`. Select Python 3.7 as runtime.

In the Lambda function, start by deleting the AWS generated default lines of code, then paste in the starter code provided in [lambda_function.py](Starter_Files/lambda_function.py) and complete the `recommend_portfolio()` function by following these guidelines:

##### User Input Validation

* The `age` should be greater than zero and less than 65.
* the `investment_amount` should be equal to or greater than 5000.

##### Investment Portfolio Recommendation

Once the intent is fulfilled, the bot should response with an investment recommendation based on the selected risk level as follows:

* **none:** "100% bonds (AGG), 0% equities (SPY)"
* **very low:** "80% bonds (AGG), 20% equities (SPY)"
* **low:** "60% bonds (AGG), 40% equities (SPY)"
* **medium:** "40% bonds (AGG), 60% equities (SPY)"
* **high:** "20% bonds (AGG), 80% equities (SPY)"
* **very high:** "0% bonds (AGG), 100% equities (SPY)"


Once I finished coding lambda function, I tested it using the Test_files.

After successfully testing the code, open the Amazon Lex Console and navigate to the `RecommendPortfolio` bot configuration, integrate the new lambda function by selecting it in the _Lambda initialization and validation_ and _Fulfillment_ sections. Build the bot, and the conversation should be as follows.

![Robo Advisor test](RobAdvisor_video.gif)


---



