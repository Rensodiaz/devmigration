---
title: "Formularios rapidos con formspree"
date: 2020-09-27T22:02:54-04:00
author: "Renso Diaz"
draft: false
 
tags: ['Express', 'HTML', 'Restify', 'Angular']
 
noSummary: false
 
categories: ['Articulos']
 
resizeImages: false
---
Crear formularios en un proceso fácil, pequeños cambios aquí y allá, estilo, validación de campos, servicios de backend y entre otros pero en este caso hablaremos sobre el procesamiento del formulario y cómo hacer llegar la información.
 
Cuando creamos nuestro formulario necesitamos un desarrollar servicios de backend para ayudarnos a enviar esos mensajes a una dirección de correo electrónica especificada, no es difícil de hacer pero existen algunas cosas que nos quitan un poco de tiempo, como la elección de un framework **ExpressJS**, **Spring boot**, **Restify**, etc. También eligiendo una infraestructura que nos corra nuestros servicios de backend y si quieres almacenar los mensajes también necesitarás crear una base de datos, vuelvo y lo repito no es imposible pero requiere tiempo.
 
 
Por todas esas razones en este tutorial usaremos un servicio que nos ayuda con todo lo mencionado anteriormente llamado [**formspree**](www.formspree.com), estos servicios incluyen enviar mensajes a destinatario específico(Se puede seleccionar más de uno), restringir envíos por dominio, integración HTML, AJAX, React y agregación de archivos, entre otras más funcionalidades.
 
Primero necesitamos registrar una cuenta con [**formspree**](www.formspree.com) y verificar el correo electrónico asociado con la cuenta registrada.
 
Luego podemos elegir que es lo que necesitamos hacer, aqui les dejo algunos ejemplos:
 
 
**Solo HTML:**
 
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
**Nota:** Este es solo un ejemplo, puedes agregar tantos campos como desees.
 
**HTML con un entorno de desarrollo como Angular 2+:**
 
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
Estos métodos son sumamente fáciles y rápido de implementar pero cuando se procesa el formulario te redireccionan a una pagina publica de [**formspree**](www.formspree.com), si eso no es un problema para tus necesidades entonces los métodos mencionados arriba serán más que suficiente. Pero si eres como yo y no quiere que los usuarios sean redireccionados a ningún lado entonces sigue leyendo porque nosotros tenemos que validar y manejar el proceso antes de enviarlo a [**formspree**](www.formspree.com).
 
 
**Manejo y validación de formularios en Angular 2+ con formspree:**
 
Para esta sección podemos seguir con la integración que hicimos en el paso anterior usando Angular, haremos algunos cambios al formulario empezando por cambiar las directivas **ngNoForm** **[action]** y **method** de nuestro formulario.
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
En este nuevo formulario usamos las directivas **[formGroup]** y **ngSubmit**.
En nuestro archivo **TS** validamos los campos utilizando Angular **FormBuilder** import:
```typescript
contactForm = this.fb.group({
   name: ['', Validators.required],
   email: ['', Validators.required],
   message: ['', Validators.required]
 });
```
Método para procesar nuestro formulario.
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
Como puedes notar en la parte de procesamiento del formulario estamos usando un servicio, este servicio es usado para hacer las peticiones **HTTP**. No necesitas un servicio para hacer peticiones http en Angular pero es una buena práctica no hacer peticiones **HTTP** en el hilo principal de nuestra aplicación, debido a que podemos ocasionar que nuestra aplicación no responda con fluidez y crear una experiencia para el usuario no adecuada.
 
**Servicio:**
 
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
 
 
**Conclucion:**
 
Como puedes ver el proceso de integrar y procesar formulario en tu aplicación web es reducido por mucho. En el futuro trabajaremos con el procesamiento de formulario nosotros mismos sin la ayuda de terceros.
