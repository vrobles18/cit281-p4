# cit281-p4

p4-module.js
// Import initial static data
const { data } = require("./p4-data.js");

// Export functions - hoisted
module.exports = {
  getQuestion,
  getAnswer,
  getQuestionAnswer,
  getQuestions,
  getAnswers,
  getQuestionsAnswers,
  addQuestionAnswer, // Extra credit
  updateQuestionAnswer, // Extra credit
  deleteQuestionAnswer, // Extra credit
};

function pluck(array, key) {
  return array.map((item) => item[key]);
}

function getQuestions() {
  /*
  // Manual method
  const questions = [];
  for (const qa of data) {
    questions.push(qa.question);
  }

  return questions;
  */
  // ES5 technique
  return pluck(data, "question");
}

function getAnswers() {
  return pluck(data, "answer");
}

function getQuestionsAnswers() {
  // This method will return a copy of the original data, which is then
  // subject to modification
  // return data;
  // Use the following technique with the current data to accomplish
  // a deep clone
  return data.map((item) => {
    return { ...item };
  });
}

function getQuestion(number = "") {
  // Prepare default response object
  const response = {
    error: "",
    question: "",
    number: "",
  };

  // Validate question number
  if (!Number.isInteger(number)) {
    response.error = "Question number must be an integer";
  } else if (number < 1) {
    response.error = "Question number must be >= 1";
  } else if (number > data.length) {
    response.error = `Question number must be less than the number of questions (${data.length})`;
  } else {
    // Get question for response
    index = number - 1;
    response.number = number;
    response.question = data[index].question;
  }

  return response;
}

function getAnswer(number = "") {
  // Prepare default response object
  const response = {
    error: "",
    answer: "",
    number: "",
  };

  // Validate answer number
  if (!Number.isInteger(number)) {
    response.error = "Answer number must be an integer";
  } else if (number < 1) {
    response.error = "Answer number must be >= 1";
  } else if (number > data.length) {
    response.error = `Answer number must be less than the number of questions (${data.length})`;
  } else {
    // Get answer for response
    index = number - 1;
    response.number = number;
    response.answer = data[index].answer;
  }

  return response;
}

function getQuestionAnswer(number = "") {
  // Get question and answer objects
  const question = getQuestion(number);
  const answer = getAnswer(number);

  // Validate question/answer objects
  if (question.error.length !== 0) {
    // Question error
    return question;
  } else if (answer.error.length !== 0) {
    // Answer error
    return answer;
  } else {
    // Both valid, combine into a response object
    return { ...question, answer: answer.answer };
  }
}

// Extra credit
function addQuestionAnswer(info = {}) {
  // Deconstruct to local variables with defaults if missing
  const { question = "", answer = "" } = info;

  // Prepare default response object
  const response = {
    error: "",
    message: "",
    number: -1,
  };

  // Validate/process object and object properties
  if (typeof info !== "object") {
    response.error = "Object with question and answer properties required";
  } else if (question.length === 0) {
    response.error = "Object question property required";
  } else if (answer.length === 0) {
    response.error = "Object answer property required";
  } else {
    // Add question/answer, and update response
    data.push({ question, answer });
    response.message = "Question added";
    response.number = data.length;
  }

  return response;
}

function updateQuestionAnswer(info = {}) {
  // Deconstruct to local variables with defaults if missing
  const { number = "", question = "", answer = "" } = info;

  // Prepare default response object
  const response = {
    error: "",
    message: "",
    number: "",
  };

  // Validate/process object and object properties
  if (typeof info !== "object") {
    response.error = "Object with question and answer properties required";
  } else if (question.length === 0 && answer.length === 0) {
    response.error = "Object question property or answer property required";
  } else if (!Number.isInteger(number)) {
    response.error = "Object number property must be a valid integer";
  } else {
    // Validate/process properties
    const index = parseInt(number) - 1;
    if (!Number.isInteger(number)) {
      response.error = "Question/answer number must be an integer";
    } else if (number < 1) {
      response.error = "Question/answer number must be >= 1";
    } else if (number > data.length) {
      response.error = `Question/answer number must be less than the number of questions (${data.length})`;
    } else {
      // Update question/answer, and update response
      const index = parseInt(number) - 1;
      data[index].question = question;
      data[index].answer = answer;
      response.message = `Question ${number} updated`;
      response.number = number;
    }
  }

  return response;
}

function deleteQuestionAnswer(number) {
  // Prepare default response object
  const response = {
    error: "",
    message: "",
    number: "",
  };

  // Validate number
  if (!Number.isInteger(number)) {
    response.error = "Question/answer number must be an integer";
  } else if (number < 1) {
    response.error = "Question/answer number must be >= 1";
  } else if (number > data.length) {
    response.error = `Question/answer number must be less than the number of questions (${data.length})`;
  } else {
    // Delete question/answer, update response
    const index = parseInt(number) - 1;
    data.splice(index, 1);
    response.message = `Question ${number} deleted`;
    response.number = number;
  }

  return response;
}

/*****************************
  Module function testing
******************************/
function testing(category, ...args) {
  console.log(`\n** Testing ${category} **`);
  console.log("-------------------------------");
  for (const o of args) {
    console.log(`-> ${category}${o.d}:`);
    console.log(o.f);
  }
}

// Set a constant to true to test the appropriate function
const testGetQs = false;
const testGetAs = false;
const testGetQsAs = false;
const testGetQ = false;
const testGetA = false;
const testGetQA = false;
const testAdd = false;      // Extra credit
const testUpdate = false;   // Extra credit
const testDelete = true;   // Extra credit

// getQuestions()
if (testGetQs) {
  testing("getQuestions", { d: "()", f: getQuestions() });
}

// getAnswers()
if (testGetAs) {
  testing("getAnswers", { d: "()", f: getAnswers() });
}

// getQuestionsAnswers()
if (testGetQsAs) {
  testing("getQuestionsAnswers", { d: "()", f: getQuestionsAnswers() });
}

// getQuestion()
if (testGetQ) {
  testing(
    "getQuestion",
    { d: "()", f: getQuestion() },      // Extra credit: +1
    { d: "(0)", f: getQuestion(0) },    // Extra credit: +1
    { d: "(1)", f: getQuestion(1) },
    { d: "(4)", f: getQuestion(4) }     // Extra credit: +1
  );
}

// getAnswer()
if (testGetA) {
  testing(
    "getAnswer",
    { d: "()", f: getAnswer() },        // Extra credit: +1
    { d: "(0)", f: getAnswer(0) },      // Extra credit: +1
    { d: "(1)", f: getAnswer(1) },
    { d: "(4)", f: getAnswer(4) }       // Extra credit: +1
  );
}

// getQuestionAnswer()
if (testGetQA) {
  testing(
    "getQuestionAnswer",
    { d: "()", f: getQuestionAnswer() },    // Extra credit: +1
    { d: "(0)", f: getQuestionAnswer(0) },  // Extra credit: +1
    { d: "(1)", f: getQuestionAnswer(1) },
    { d: "(4)", f: getQuestionAnswer(4) }   // Extra credit: +1
  );
}

// Extra credit
// addQuestionAnswer()
if (testAdd) {
  testing(
    "addQuestionAnswer",
    { d: "()", f: addQuestionAnswer() },
    { d: "({})", f: addQuestionAnswer({}) },
    { d: '(question: "Q4")', f: addQuestionAnswer({ question: "Q4" }) },
    { d: '(answer: "A4")', f: addQuestionAnswer({ answer: "A4" }) },
    {
      d: '(question: "Q4", answer: "A4")',
      f: addQuestionAnswer({ question: "Q4", answer: "A4" }),
    }
  );
}

// updateQuestionAnswer()
if (testUpdate) {
  testing(
    "updateQuestionAnswer",
    { d: "()", f: updateQuestionAnswer() },
    { d: "({})", f: updateQuestionAnswer({}) },
    { d: '(question: "Q1U")', f: updateQuestionAnswer({ question: "Q1U" }) },
    { d: '(answer: "A1U")', f: updateQuestionAnswer({ answer: "A1U" }) },
    {
      d: '(question: "Q1U", answer: "A1U")',
      f: updateQuestionAnswer({ question: "Q1U", answer: "A1U" }),
    },
    {
      d: '(number: 1, question: "Q1U", answer: "A1U")',
      f: updateQuestionAnswer({ number: 1, question: "Q1U", answer: "A1U" }),
    }
  );
  console.log(data);
}

// deleteQuestionAnswer()
if (testDelete) {
  testing(
    "deleteQuestionAnswer",
    { d: "()", f: deleteQuestionAnswer() },
    { d: "(0)", f: deleteQuestionAnswer(0) },
    { d: "(1)", f: deleteQuestionAnswer(1) },
    { d: "(0)", f: deleteQuestionAnswer(4) }
  );
  console.log(data);
}
p4-server.js
const fastify = require("fastify")();
const {
  getQuestion,
  getAnswer,
  getQuestions,
  getAnswers,
  getQuestionsAnswers,
  addQuestionAnswer,      // Extra credit
  updateQuestionAnswer,   // Extra credit
  deleteQuestionAnswer    // Extra credit
} = require("./p4-module.js");

fastify.get("/cit/question", (request, reply) => {
  // Return response
  reply
    .code(200)
    .header("Content-Type", "text/json; charset=utf-8")
    .send({ error: "", statusCode: 200, questions: getQuestions() });
});

fastify.get("/cit/question/:number", (request, reply) => {
  // Extract question number using deconstruction
  let { number = "" } = request.params;

  // Setup default response object
  const response = {
    error: "",
    statusCode: 200,
    question: "",
    number: "",
  };

  // Process request
  if (number === "") {
    // Question number was not specified
    response.error = "Number route parameter required";
    response.statusCode = 404;
  } else {
    // Valid route parameter, convert to number
    number = parseInt(number);

    // Get question
    const questionInfo = getQuestion(number);

    // Check response
    if (questionInfo.error.length > 0) {
      // Error getting question
      response.error = questionInfo.error;
      response.statusCode = 404;
    } else {
      // Valid question found and returned
      response.question = questionInfo.question;
      response.number = number;
    }
  }

  // Return response
  reply
    .code(response.statusCode)
    .header("Content-Type", "text/json; charset=utf-8")
    .send(response);
});

fastify.get("/cit/answer", (request, reply) => {
  // Return response
  reply
    .code(200)
    .header("Content-Type", "text/json; charset=utf-8")
    .send({ error: "", statusCode: 200, answers: getAnswers() });
});

fastify.get("/cit/answer/:number", (request, reply) => {
  // Extract answer number using deconstruction
  let { number = "" } = request.params;

  // Setup default response object
  const response = {
    error: "",
    statusCode: 200,
    answer: "",
    number: "",
  };

  // Process request
  if (number === "") {
    // Answer number was not specified
    response.error = "Number route parameter required";
    response.statusCode = 404;
  } else {
    // Valid route parameter, convert to number
    number = parseInt(number);

    // Get answer
    const answerInfo = getAnswer(number);

    // Check response
    if (answerInfo.error.length > 0) {
      // Error getting answer
      response.error = answerInfo.error;
      response.statusCode = 404;
    } else {
      // Valid answer found and returned
      response.answer = answerInfo.answer;
      response.number = number;
    }
  }

  // Return response
  reply
    .code(response.statusCode)
    .header("Content-Type", "text/json; charset=utf-8")
    .send(response);
});

fastify.get("/cit/questionanswer/:number", (request, reply) => {
  // Extract question/answer number using deconstruction
  let { number = "" } = request.params;

  // Setup default response object
  const response = {
    error: "",
    statusCode: 200,
    question: "",
    answer: "",
    number: "",
  };

  // Process request
  if (number === "") {
    // Answer number was not specified
    response.error = "Number route parameter required";
    response.statusCode = 404;
  } else {
    // Valid route parameter, convert to number
    number = parseInt(number);

    // Get answer
    const answerInfo = getAnswer(number);

    // Check response
    if (answerInfo.error.length > 0) {
      // Error getting answer
      response.error = answerInfo.error;
      response.statusCode = 404;
    } else {
      // Valid answer found and returned
      response.answer = answerInfo.answer;
      response.number = number;
    }
  }

  // Return response
  reply
    .code(response.statusCode)
    .header("Content-Type", "text/json; charset=utf-8")
    .send(response);
});

fastify.get("/cit/questionanswer", (request, reply) => {
  // Return response
  reply.code(200).header("Content-Type", "text/json; charset=utf-8").send({
    error: "",
    statusCode: 200,
    questions_answers: getQuestionsAnswers(),
  });
});

// Handle unmatched routes
fastify.get("*", (request, reply) => {
  reply
    .code(404)
    .header("Content-Type", "text/json; charset=utf-8")
    .send({ error: "Route not found", statusCode: 404 });
});

// Extra credit routes post, put, and delete
fastify.post("/cit/question", (request, reply) => {
  // Setup default response object
  const response = {
    error: "",
    statusCode: 201,
    number: "",
  };

  // Get/handle question/answer add
  const qa = addQuestionAnswer(request.body);
  if (qa.error.length > 0) {
    // Handle error
    response.statusCode = 409;
    response.error = qa.error;
  } else {
    // Handle valid add
    response.number = qa.number;
  }

  // Return response
  reply
    .code(response.statusCode)
    .header("Content-Type", "text/json; charset=utf-8")
    .send(response);
});

fastify.put("/cit/question/:number", (request, reply) => {
  // Decontruct to get question number
  let { number = "" } = request.params;

  // Setup default response object
  const response = {
    error: "",
    statusCode: 200,
    number: "",
  };

  // Process request
  if (number === "") {
    // Question number was not specified
    response.error = "Number route parameter required";
    response.statusCode = 404;
  } else {
    // Valid route parameter, convert to number
    number = parseInt(number);

    // Get/handle question/answer update
    const qa = updateQuestionAnswer({number, ...request.body});
    if (qa.error.length > 0) {
      // Handle error
      response.statusCode = 409;
      response.error = qa.error;
    } else {
      // Handle valid update
      response.number = qa.number;
    }
  }
  // Return response
  reply
    .code(response.statusCode)
    .header("Content-Type", "text/json; charset=utf-8")
    .send(response);
});

fastify.delete("/cit/question/:number", (request, reply) => {
  // Decontruct to get question number
  let { number = "" } = request.params;

  // Setup default response object
  const response = {
    error: "",
    statusCode: 200,
    number: "",
  };

  // Process request
  if (number === "") {
    // Question number was not specified
    response.error = "Number route parameter required";
    response.statusCode = 404;
  } else {
    // Valid route parameter, convert to number
    number = parseInt(number);

    // Get/handle question/answer delete
    const qa = deleteQuestionAnswer(number);
    if (qa.error.length > 0) {
      // Handle error
      response.statusCode = 409;
      response.error = qa.error;
    } else {
      // Handle valid update
      response.number = qa.number;
    }
  }
  // Return response
  reply
    .code(response.statusCode)
    .header("Content-Type", "text/json; charset=utf-8")
    .send(response);
});

// Start server and listen to requests using Fastify
const listenIP = "localhost";
const listenPort = 8080;
fastify.listen(listenPort, listenIP, (err, address) => {
  if (err) {
    console.log(err);
    process.exit(1);
  }
  console.log(`Server listening on ${address}`);
});
