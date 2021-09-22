---
layout: page
title: Contact
---

<form accept-charset="UTF-8" action="https://formspree.io/xrgyojkl" method="POST" enctype="multipart/form-data" target="_blank">
          <div class="form-group">
            <label for="exampleInputEmail1" required="required">Email address</label>
            <input type="email" name="email" class="form-control" id="input-email" aria-describedby="emailHelp" placeholder="Enter email">
          </div>
          <div class="form-group">
            <label for="exampleInputName">Name</label>
            <input type="text" name="name" class="form-control" id="input-name" placeholder="Enter your name" required="required">
          </div>
          <div class="form-group">
            <label for="subject">Subject</label>
            <div class="form-select-wrap">
              <select class="form-control" id="subject" name="subject">
                <option value="">Please select</option>
                <option value="Error on the site">Error on the site</option>
                <option value="Sponsorship">Sponsorship</option>
                <option value="Other">Other</option>
              </select>
            </div>
          </div>
          <div class="form-group">
            <label for="message">Message</label>
            <br>
            <textarea name="message" id="message" rows="7" placeholder="Your message" class="form-control"></textarea>
          </div>
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="checkbox" id="consent" value="agree">
            <label class="form-check-label" for="inlineCheckbox1">I understand that this form is storing my submitted information so I can be contacted.</label>
          </div>
          <hr>
          <button type="submit" class="btn btn-primary">Submit</button>
        </form>
