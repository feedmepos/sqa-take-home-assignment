## FeedMe Software Quality Assurance Take-Home Assignment
Below is a take-home assignment before the interview for the position. You are required to
1. Understand the situation and use case. You may contact the interviewer for further clarification.
2. Develop test cases for each user story using your most efficient tools.
3. Run your test case against the prototype to capture bugs in the prototype.
4. Document and analyze your test result.
5. Bring your source code and test result to the next interview session.

### Situation
McDonald is transforming their business during COVID-19. They wanted to build automated cooking bots to reduce human costs and increase efficiency. As one of the software quality assurance in the project, your task is to ensure the prototype being developed is free from bugs using various testing strategies.

### User Story
Below is the user story capture from McDonald, please develop test cases for each user story.
- 01: As a McDonald's customer, after I submitted my order, I want to see my order appear in the "PENDING" area.
- 02: As a McDonald's customer, after a cooking bot processed my order, I want to see my order moved to the "COMPLETE" area.
- 03: As a McDonald's VIP member, after I submitted my order, I want the cooking bot to process my order first before all non-VIP orders.
- 04: As the McDonald's manager, I want orders of the same type (VIP and non-VIP) to be processed according to the "First come First serve" rule.
- 05: As the McDonald's manager, I want to increase and decrease the number of cooking bots available in my restaurant.
- 06: When a McDonald's cooking bot is added to the restaurant, it should start working on the order immediately.
- 07: When a McDonald's cooking bot is removed from the restaurant, it should abandon its order immediately and release it to other bots (if any).
- 08: A McDonald's cooking bot can only process one order at a time, and required 3 seconds to complete an order.

### Test against pototype
Our development team has developed a prototype [here](https://nervous-mcclintock-523688.netlify.app). Please run your test case against the prototype, and capture any bug. For any bug found, document and analyze the bug so that the development team can resolve it effectively.

### Test result
Your test and test result must meet the following criteria:
- precisely address the need user story
- executable (manual or automatic)
- readable for various audience, eg: project manager, development team

### Tips on completing this assignment
- Use the best tools you have on hand.
- Try to scope your working hour within 3 hours (1 hour per day if you really busy) and avoid unnecessary optimization and documentation.
- Communicate effectively like you are going to communicate with the actual team member.
