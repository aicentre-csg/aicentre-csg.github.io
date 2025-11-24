---
title: "Contact Us"
layout: default
permalink: /contact/
---

# Contact Us

We are always looking for new collaborations and opportunities. Please reach out to us via the details below or use the form to send a message.

---

### Send us a Message

<style>
.contact-form-container { max-width: 600px; margin-top: 20px; }
.form-group { margin-bottom: 20px; }
.form-label { display: block; margin-bottom: 8px; font-weight: 600; color: #333; }
.form-input { width: 100%; padding: 12px; border: 1px solid #ccc; border-radius: 6px; box-sizing: border-box; font-size: 16px; font-family: inherit; transition: border-color 0.3s ease; }
.form-input:focus { outline: none; border-color: #0056b3; box-shadow: 0 0 5px rgba(0, 86, 179, 0.2); }
.submit-btn { background-color: #333; color: white; padding: 12px 24px; border: none; border-radius: 6px; cursor: pointer; font-size: 16px; font-weight: 600; transition: background-color 0.3s ease; }
.submit-btn:hover { background-color: #555; }
</style>

<div class="contact-form-container">
<form action="https://formspree.io/f/mgvddlkd" method="POST">
<div class="form-group">
<label for="name" class="form-label">Your Name</label>
<input type="text" id="name" name="name" class="form-input" placeholder="John Doe" required>
</div>
<div class="form-group">
<label for="email" class="form-label">Your Email</label>
<input type="email" id="email" name="_replyto" class="form-input" placeholder="john@example.com" required>
</div>
<div class="form-group">
<label for="message" class="form-label">Message</label>
<textarea id="message" name="message" class="form-input" rows="6" placeholder="How can we help you?" required></textarea>
</div>
<button type="submit" class="submit-btn">Send Message</button>
</form>
</div>