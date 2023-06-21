library(faq)
df <- data.frame(
  question = c("Question1", "Question2", "Question3"),
  answer = c("answer for question1", 
             "question2 answer", 
             "answer3")
)
faq::faq(data = df, elementId = "faq", faqtitle = "Frequently Asked Questions")
