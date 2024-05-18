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

<form id="contactForm">
  <input type="text" id="name" name="name" placeholder="Your name" required>
  <input type="text" id="email" name="email" placeholder="Your email" required>
  <textarea rows="5" id="message" name="message" placeholder="Your message" required></textarea>
  <input type="button" value="[ Submit ]" onclick="validateAndEncryptForm()">
</form>

<div id="successMessage"></div>
<div id="errorMessage"></div>

<script src="https://cdn.rawgit.com/travist/jsencrypt/master/bin/jsencrypt.min.js"></script>
<script>
  const PUBLIC_KEY = `-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoiQ4oVwCDdwGy5Eq1x3e
ak4/hzEZi73KgP0MEaNTStmfsmC89uhQ4wmDHacv04pZcjsFwgG3C/ff0UucM66r
vXhWBZCRoFHcekbd4+4RlEwC12+LkgrCdwwCmRH8B+GmFYyyMNdjKyMEENlMwt+Y
e39nBQ20XxJyFb023mNohy4HidZ9XaX7TsqVKqqJKmYBoZULAxq1bSBRI+9T/0PI
nervu8aF5ch2bjGXXkHxrk77mFDZJ+9EGnEIS0dEdmeRBO9DSQzvgK9sJZhftKKl
xA4Orwqk6el5iNXrg0JOA9IeMFK3KGb9+GP2m8SaVrS6881aa/Lrt2r9zRaE5iY4
OwIDAQAB
-----END PUBLIC KEY-----`;

  function validateAndEncryptForm() {
    var emailInput = document.getElementById('email');
    var messageInput = document.getElementById('message');
    var errorMessage = document.getElementById('errorMessage');
    var successMessage = document.getElementById('successMessage');

    var emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(emailInput.value)) {
      errorMessage.textContent = 'Invalid email address';
      successMessage.textContent = '';
      return false;
    }

    var encrypt = new JSEncrypt();
    encrypt.setPublicKey(PUBLIC_KEY);
    var encryptedMessage = encrypt.encrypt(messageInput.value);

    if (!encryptedMessage) {
      errorMessage.textContent = 'Encryption failed';
      successMessage.textContent = '';
      return false;
    }

    // Create a form to submit the encrypted message
    var form = document.createElement('form');
    form.method = 'POST';
    form.action = 'https://formspree.io/f/xqkrppre';

    var nameField = document.createElement('input');
    nameField.type = 'hidden';
    nameField.name = 'name';
    nameField.value = document.getElementById('name').value;
    form.appendChild(nameField);

    var emailField = document.createElement('input');
    emailField.type = 'hidden';
    emailField.name = 'email';
    emailField.value = emailInput.value;
    form.appendChild(emailField);

    var messageField = document.createElement('input');
    messageField.type = 'hidden';
    messageField.name = 'message';
    messageField.value = encryptedMessage;
    form.appendChild(messageField);

    document.body.appendChild(form);
    form.submit();

    errorMessage.textContent = '';
    successMessage.textContent = 'Message sent successfully!';
    document.getElementById('contactForm').reset();
  }
</script>

