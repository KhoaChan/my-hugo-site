---
title: "Week 11 Worklog"
date: 2026-06-26
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---


## I. Executive Summary

After completing the overall system architecture design in the previous week, in Week 8 I continued moving into the technical design refinement stage before starting the first implementation activities of the internship topic. The main objective of this week was to review the entire architecture, analyze each system component in more detail, and identify how each module could be implemented during the development process.

During the research process, I continued referring to AWS Documentation, AWS Prescriptive Guidance, and several Video-on-Demand architecture models on AWS to evaluate whether the selected architecture was suitable. Through self-review and analysis, I adjusted several components to improve scalability, reduce dependencies between services, and make the implementation process more practical.

At the same time, I started designing the data model for Amazon DynamoDB, building a list of business APIs expected to be implemented through Amazon API Gateway and AWS Lambda, and defining the data flow between system components.

By the end of the week, I had completed the basic technical design documents, creating a foundation for moving into the system implementation stage in the following week.

---

## II. Strategic Objectives

After defining the overall architecture, the objective of Week 8 was to make the technical components more specific in preparation for system development.

The main objectives included:

- Review and refine the overall architecture based on research and analysis.
- Design the data model for Amazon DynamoDB.
- Identify business APIs and the communication method between the Frontend and Backend.
- Prepare the project structure and divide the system into functional modules.
- Build a detailed implementation plan for each development phase.
- Ensure that design decisions meet requirements for scalability, performance, and operational cost.

Through these objectives, I aimed to create a consistent technical foundation before starting the first development activities of the system.

---

## III. Activity Log & Detailed Schedule Allocation (From 09/06/2026 to 15/06/2026)

| Time | Activity Category | Detailed Tasks Performed | Results / Evidence Achieved |
| --- | --- | --- | --- |
| Day 1 (30/06) | Report Content Review | Reviewed all worklog sections from previous weeks and checked whether the content matched the internship progress. | Identified sections that needed editing or additional evidence. |
| Day 2 (01/07) | Blog Content Completion | Reviewed and edited technical blog posts related to AWS services, Serverless Architecture, Aurora DSQL, and S3 configuration replication. | Completed the main technical blog content. |
| Day 3 (02/07) | Workshop and Event Report | Updated the workshop and event participation sections, summarized learning outcomes, and prepared image placeholders. | Completed the workshop and event report sections. |
| Day 4 (03/07) | Image and Evidence Check | Checked image paths, screenshots, Markdown image syntax, and the location of files in the Hugo static folder. | Fixed several image path and formatting issues. |
| Day 5 (04/07) | Markdown and Hugo Formatting | Reviewed headings, tables, code blocks, front matter, menu weight, and bilingual page structure. | Improved the visual format and navigation structure of the report website. |
| Day 6 (05/07) | Self-Evaluation Section | Wrote and refined the self-evaluation section, including personal strengths, weaknesses, and areas for improvement. | Completed the self-evaluation content. |
| Day 7 (06/07) | Weekly Summary | Reviewed the overall report structure and prepared the remaining sections for final completion. | Completed Week 11 report review and formatting updates. |


---

## IV. Detailed Work Execution & Analysis

In Week 11, I mainly focused on reviewing and improving the content of my internship report. After completing the worklogs, technical content, and project-related sections in the previous weeks, I started checking the entire report structure to ensure that the content matched my internship progress and presentation requirements.

First, I reviewed all Worklog sections from the previous weeks. The purpose of this task was to check whether each week accurately reflected my learning process, practice activities, and project development progress. Sections that lacked explanations, screenshots, or clear descriptions were noted for further revision.

In addition, I continued improving the technical blog posts related to AWS, Serverless Architecture, Aurora DSQL, and Amazon S3 configuration replication. These posts were reviewed in terms of content, structure, technical terminology, and presentation style to make them more suitable for the internship report.

Besides the blog section, I also updated the Workshop and Events Participated sections. I added event summaries, learning outcomes, and image placeholders. This helped the report show not only theoretical learning but also practical activities that I participated in during the program.

Another important task during this week was checking images and evidence. I reviewed image paths, Markdown image syntax, the location of files inside the Hugo static folder, and how images were displayed on the website. Some image path and formatting issues were identified and fixed.

I also spent time checking the Markdown/Hugo formatting, including headings, tables, code blocks, front matter, menu weight, and bilingual page structure. These adjustments helped the report website look clearer and made the navigation easier to use.

Finally, I wrote and refined the Self-Evaluation section. This section focused on evaluating my strengths, weaknesses, and the skills I had developed during the internship.

---

## V. Challenges, Error Handling & Solutions

In Week 11, the main challenge was not implementing new technical features but reviewing and standardizing the entire report. Since the report contained many sections written across different weeks, I noticed that some parts were not consistent in writing style, headings, table format, and image presentation.

A common issue was that some image paths in Markdown did not match the actual file locations inside the `static/images` folder. This caused some images to open correctly through direct links but not display properly inside the content pages. To solve this, I checked the folder structure, corrected the image paths according to the website baseURL, and rebuilt the Hugo site for verification.

In addition, some tables and code blocks had formatting errors because of missing closing lines or incorrect copied Markdown syntax. I reviewed the table sections, code blocks, and headings to ensure that Hugo rendered the pages correctly.

Maintaining both English and Vietnamese versions also created some difficulties. If one version was updated but the other was not, the content could become inconsistent. Therefore, I checked both language versions side by side to make sure their structure and main ideas remained aligned.

---

## VI. Professional Reflections

Week 11 helped me realize that completing an internship report is just as important as doing technical practice. A good report does not only need complete content, but also requires a clear structure, suitable evidence, and an easy-to-follow presentation.

By reviewing the blog posts, workshops, events, and worklogs, I had the opportunity to look back at my entire internship process. This helped me systematize the knowledge I had learned, the skills I had practiced, and the areas that still needed improvement before final submission.

In addition, editing Markdown/Hugo content helped me better understand how to organize documentation in a website format. I learned how to check front matter, weight, image paths, bilingual structure, and how to fix display issues on a Hugo website.

Overall, Week 11 was an important stage for standardizing content, fixing presentation issues, and preparing a solid foundation for the final report completion in Week 12.

---

## VII. Plan for Next Week

In the following week, I will focus on completing the final version of the internship report. The main tasks will include reviewing all content one last time, completing the Feedback section, checking spelling, verifying evidence images, and preparing the report for submission.

I will also test the Hugo website again after the updates to ensure that links, menus, images, and bilingual content are displayed correctly. After that, I will prepare the final version for GitHub deployment and use it for the internship report submission.