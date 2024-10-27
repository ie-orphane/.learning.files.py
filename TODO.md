# Random Quiz

Say you’re a geography teacher with **35 students** in your class and you want to give a pop quiz on capitals of countries.

Alas, your class has a few bad eggs in it, and you can’t trust the students not to cheat.

You’d like to randomize the order of questions so that each quiz is unique, making it impossible for anyone to crib answers from anyone else.

Of course, doing this by hand would be a lengthy and boring affair.

Fortunately, you know some Python.


Here is what the program does:

- Creates 35 different quizzes.
- Creates 25 multiple-choice questions for each quiz, in random order.
- Provides the correct answer and three random wrong answers for each question, in random order.
- Writes the quizzes to 35 text files.


This means the code will need to do the following:

- Read the countries and the students data.
- Use open(), write(), or print() for the quiz and answer key text files.
- Use random.shuffle() to randomize the order of the questions and multiple-choice options.


### Quiz Template:

```txt

ID   : {STUDENT_ID}
NAME : {STUDENT_NAME}
DATE : {TODAY_DATE}


What is the capital of ?


{QUESTION_NUMBER}. {COUNTRY}
    [ ] {ANSWER_0}
    [ ] {ANSWER_1}
    [ ] {ANSWER_2}
    [ ] {ANSWER_3}

```

### Note:

- You must work with **OOP**.
- The quiz file will be created in **`assets/quizes/`** and named by **student id**.
- The **`quizes`** directory should be **generated** every time the program runs.
