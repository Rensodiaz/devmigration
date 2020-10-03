---
title: "Forms made easy with formspree"
date: 2020-09-27T22:02:54-04:00
author: "Renso Diaz"
draft: false
 
tags: ['Express', 'HTML', 'Restify', 'Angular']
 
noSummary: false
 
categories: ['Articles']
 
resizeImages: false
---
Making forms is a very straightforward process, small changes here and there, style, fields validations, backend services and so on but in our case we would talk about the extra step of processing the forms and the delivery of the messages.
 
When we are trying to create a form for our website we have to also develop a backend service to help us send those messages to a specific email address, it’s not difficult to achieve but there are a couple of things to do that kind of take a lot of time, for example: Selecting a backend framework like **ExpressJS**, **Spring boot**, **Restify**, etc. Also choosing an infrastructure to run your backend service and if you want to store those messages yourself you will have to also deal with a database, again any of these processes are feasible but time consuming.
 
That’s why for this tutorial we are going to use a service called [**formspree**](www.formspree.com) which does all of those things for us, like forwarding messages to a selected email address(we can choose more than one), restricting submissions by Domain, HTML, AJAX, React, and File upload integration and many more features.
 
First you need to register an account with [**formspree**](www.formspree.com) and verify the email address associated with the account.
 
Then we can choose what exactly are our needs, here are some examples:
 
**HTML without any framework:**
 
```HTML
<form action="your.formspree.url/here" method="POST">
 <label>
   Your email:
   <input type="text" name="name">
 </label>
 <label>
   Your message:
   <textarea name="message"></textarea>
 </label>
 <button type="submit">Send</button>
</form>
```
**Note:** This is just an example but you can have as many fields as you like.
 
**HTML with a framework like Angular 2+:**
 
```typescript
<form ngNoForm [action]="formspreeUrl" method="POST">
      <label>
          Name:
          <input type="text" name="name">
      </label>
      <label>
          Email:
          <input type="email" name="email">
      </label>
      <br>
      <label>
          Your message:
          <textarea name="message"></textarea>
      </label>
      <br>
      <button type="submit">Send</button>
  </form>
```
 
These methods are very straightforward and fast but it will redirect you to a [**formspree**](www.formspree.com) landing page, if that's not an issue for you, any of the methods from above should get you going but if you are like me you don't want the user to go anywhere, you will validate and handle the form yourself.
 
 
**Handling validation and processing of forms with Angular2+ formspree:**
 
For this section we can keep using the form we used for Angular, the only things we will need to change is the directives that we are using like  **ngNoForm** **[action]** and **method** to process the form.
```typescript
<div class="contact">
   <form [formGroup]="contactForm" (ngSubmit)="onSubmit()">
       <div class="row">
           <div class="column">
               <input type="text" placeholder="Name" formControlName="name" required>
           </div>
           <div class="column">
               <input id="email" type="email" placeholder="Email" formControlName="email" required (keyup)="checkEmail($event)">
               <p class="invalid-email" *ngIf="!invalidEmail">*Please enter a valid email.</p>
           </div>
       </div>
       <br>
       <br>
       <textarea type="text" class="multi-line" placeholder="Message" formControlName="message" required></textarea>
       <br>
       <br>
       <button class="btn-send" type="submit" [disabled]="!contactForm.valid" [class.ready]="contactForm.valid">Send Message</button>
   </form>
</div>
```
In this form we are going to use the **[formGroup]** y **ngSubmit**. Now let’s switch to the **TS** file here we can validate the field using **FormBuilder** import:
```typescript
contactForm = this.fb.group({
   name: ['', Validators.required],
   email: ['', Validators.required],
   message: ['', Validators.required]
 });
```
Method to process our form:
```typescript
/**
  * This method will help us validated the form as well
  */
 onSubmit():void {
   const email = this.contactForm.get('email').value;
   this.invalidEmail = this.regexp.test(email);
   //Get the email element to apply some style to it.
   if(!this.invalidEmail){
     this.emailDecorator(this.invalidEmail);
   }else{
    
     //Remove change border color
     this.emailDecorator(this.invalidEmail);
 
     const contact: Contact = ({
       name: this.contactForm.get('name').value,
       email: email,
       message: this.contactForm.get('message').value
     })
     //Service to handle the http POST request in the background
     this.contactService.sendEmail(contact).subscribe( res =>{
       if(res.ok){
         this.contactForm.reset();
       }else{
         console.error(res);
       }
     });
   }
 }
```
Here we can note the process and validation of the form, one thing to paid attention is to the service, the service handle the **HTTP** request. The service is not require to make this request in **Angular** but as a good practice it is not recommended to do network requests in the main thread of your application because that can cause your application to be slow while doing the request and may not be the desired user experience that you may be looking for.
 
 
 
**Service:**
 
```typescript
@Injectable({
 providedIn: 'root'
})
export class ContactService {
 
 //Formspree API
 private formUrl = 'https://formspree.io/mykeyhere';
 private headers = new HttpHeaders({'content-type': 'application/json'})
 constructor(private httpClient: HttpClient) { }
 
 /**
  * Post method to send the form data to the email we set in formspree
  * @param contact form data to be process
  */
 sendEmail(contact: Contact): Observable<any>{
   return this.httpClient.post<any>(
     this.formUrl,
   {//Post body
     name: contact.name,
     email: contact.email,
     message: contact.message
   },
   {
     headers: this.headers
   }
   ).pipe(
     tap(_ => console.warn('sending message')),
     catchError(this.handleError<any>('sendEmail', []))
   );
 }
 
 /**
  * Requests error handler
  * @param operation Type of operation we are executing
  * @param result what exactly happen after the execution
  */
 private handleError<T>(operation = 'operation', result?: T){
   return(error: any): Observable<T> =>{
     //We can stream this log to an platform like CloudWatch
     console.error(error);
 
     //Let our app keep running
     return of(result as T);
   }
 }
}
```
 
 
**Conclusion:**
 
As you can see this will cut the whole process by a lot and get your website up and running working with forms. In the future we will work in creating forms and handling everything ourselves, that way we won't depend on third party services but this is a great start.
 
 
 
 
 
 

