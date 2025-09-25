# Cursor AI Demo Generation

This project was created to help **Solutions Engineer** build industry specific demos to appeal to customers who may want to see their data used in the **Snowflake AI Platform**.

The project approach is simple, with a lot of thought put into its execution. 

# What you need to create a demo

1. Cursor AI to be installed in your laptop. The instructions where built for MacOS laptops, if you have a Windows OS, other instructions may apply.
2. Setup the Snowflake Documentation under your Cursor AI Setting -> Indexing & Docs -> Docs. This will become your context when building your prompt.
3. Snowflake Demo Account Credentials.
4. Imagination and Creativity.

## Instructions

1. Create a folder, you can name it whatever you like. 
2. Add the **cursor_ai_snowflake_demo_instructions.md** file to the folder. 
3. Open Cursor AI and open the folder you just created with the file. When prompted to “**Do you trust the authors of the files in this folder?**”, click “**Yes, I trust the authors**”.
4. Before prompting, in your AI Panel include your context before you start. (e.g., @add Context) It is recommended to include the Snowflake Documentation to guide the coding to best practices. 
5. Define your prompt either general, semi-specific, or specific for better results. It goes without saying that the more specific you are, the more facts, details you add to your prompt, the outcome will undoubtedly be more targeted to what you are looking for. 
6. Once the demo is created, you will need to update the Snowflake config file with your Snowflake credentials like Account name, Username and Password. Please have your phone handy because Duo will be prompting you and is time sensitive. 

## Things to lookout for

1. If you do not accept the Duo connection to Snowflake, it will try to hard code the data for your results in the Streamlit Application created for your dashboard. 
2. When building the synthetic data, you may guide the agent to curate better context, but it needs to be noted. Sometimes it may create data funny like instead of names when building a healthcare demo, it will say something like Patient1, Patient2, Patient3, etc. You can ask to make names up and to modify such data. 
