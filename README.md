# Cursor AI Demo Generation

This project was created to help **Solutions Engineers** build industry specific demos to appeal to customers who may want to see their data used in the **Snowflake AI Platform**.

The project approach is simple, with a lot of thought put into its execution. 

# What you need to create a demo

1. Cursor AI to be installed in your laptop. The instructions where built for MacOS laptops, if you have a Windows OS, other instructions may apply.
2. Setup the Snowflake Documentation under your Cursor AI Setting -> Indexing & Docs -> Docs. This will become your context when building your prompt.
3. Snowflake Account.
4. SnowSQL installed with a dedicated connection for Cursor AI.
5. Imagination and Creativity.

## Instructions

1. The Snowflake Connection is handled by SnowSQL. Please refer to the SE Guide using this [link](https://docs.google.com/document/d/1cj5PHPq2Jk_Xrsoimr24lRNnJSg9vYEu2H6OkEaeY7A/edit?tab=t.0#heading=h.os1muddxeqny "link").
2. Create a folder inside your projects, you can name it whatever you like. 
3. Add the **cursor_ai_snowflake_demo_instructions.md** file to the folder. 
4. Open Cursor AI and open the folder you just created with the file. When prompted to “**Do you trust the authors of the files in this folder?**”, click “**Yes, I trust the authors**”.
5. Before prompting, in your AI Panel include your context before you start. (e.g., @add Context) It is recommended to include the Snowflake Documentation to guide the coding to best practices. 
6. Define your prompt either general, semi-specific, or specific for better results. It goes without saying that the more specific you are, the more facts, details you add to your prompt, the outcome will undoubtedly be more targeted to what you are looking for. [Prompt Examples!](https://github.com/hannehbb3/Cursor_Demo_Generation/blob/main/Prompt%20Examples.md)
7. Once the demo is created, you need to supervise the agent to make sure it builds all the assets you need for a demo.
8. In some instances, rework and recoding may be required but Cursor AI is so awesome, it will take up on that challenge.
9. Once you have the outputs needed for your demo, you are welcome to elevate your demo to a different feature or type. Remember the sky is the limit here. 

## Things to lookout for

1. If you do not accept the Duo connection to Snowflake, it will try to hard code the data for your results in the Streamlit Application created for your dashboard.
2. When uploading the streamlit app to Snowflake, some recoding will need to be applied for it to work. Streamlit app on your desktop versus the streamlit app in Snowflake are NOT the same. (e.g., packages, connection, versions, etc.) 
3. When building the synthetic data, you may guide the agent to curate better context, but it needs to be noted. Sometimes it may create data funny like instead of names when building a healthcare demo, it will say something like Patient1, Patient2, Patient3, etc. You can ask to make names up and to modify such data.
4. Cursor is extremely diligent when "YOU" ask something either to respond or to do. For this remember you are dealing with the best college grad, from the best university, with Cum Laude honors for which you still need to manage.

### We encourage feedback and suggestions. Thank you!
