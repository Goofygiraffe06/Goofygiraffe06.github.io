---
layout: default
title: /contact
permalink: /contact
---

<style>
  #contactForm {
    display: flex;
    flex-direction: column;
    max-width: 400px;
    margin: auto;
  }

  label {
    margin-bottom: 0.5em;
  }

  input,
  textarea {
    padding: 0.5em;
    margin-bottom: 1em;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  input[type="submit"] {
    background-color: #4CAF50;
    color: white;
    cursor: pointer;
  }

  input[type="submit"]:hover {
    background-color: #45a049;
  }

  #successMessage {
    color: green;
    font-weight: bold;
    margin-top: 1em;
  }

  #errorMessage {
    color: red;
    font-weight: bold;
    margin-top: 1em;
  }
</style>

# Get in touch?

<form id="contactForm" action="https://formspree.io/f/xqkrppre" method="post">
  <input type="text" id="name" name="name" placeholder="Your name" required>
  <input type="text" id="email" name="email" placeholder="Your email" required>
  <textarea rows="5" id="message" name="message" placeholder="Your message" required></textarea>
  <input type="submit" value="[ Submit ]" onclick="validateForm()">
</form>

<div id="successMessage"></div>
<div id="errorMessage"></div>

<script>
  function validateForm() {
    var emailInput = document.getElementById('email');
    var errorMessage = document.getElementById('errorMessage');
    var successMessage = document.getElementById('successMessage');

    var emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(emailInput.value)) {
      errorMessage.textContent = 'Invalid email address';
      successMessage.textContent = '';
      return false;
    }

    errorMessage.textContent = '';
    successMessage.textContent = 'Message sent successfully!';
    document.getElementById('contactForm').submit();
  }
</script>

