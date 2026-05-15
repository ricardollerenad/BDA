# 🚀 Deploy Full Stack (Express + MongoDB + Angular + PM2) en VPS Debian 12

---

# 🧠 1. INSTALAR PM2 (GESTOR DE PROCESOS NODE)

sudo npm install -g pm2  
pm2 -v  
pm2 start index.js  

---

# 🌐 2. INSTALAR ANGULAR CLI + CREAR PROYECTO

sudo npm install -g @angular/cli  
ng version  
ng new frontend  
cd frontend  

---

# 🚀 3. LEVANTAR ANGULAR EN VPS

ng serve --host 0.0.0.0 --port 4200  
ng serve --host 161.132.51.98  

Acceso:
http://IP_VPS:4200  

---

# ⚙️ 4. CREAR SERVICIO ANGULAR (CRUD)

ng generate service services/user  

---

## 📌 app.config.ts (ANGULAR 20 - HTTP + ROUTER)

<pre>
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';
import { provideHttpClient } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideHttpClient()
  ]
};
</pre>

---

# 📡 5. SERVICIO CRUD (ANGULAR → EXPRESS API)

## services/user.service.ts

<pre>
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private apiUrl = 'http://IP_VPS:3000/api/users';

  constructor(private http: HttpClient) {}

  getUsers(): Observable<any> {
    return this.http.get(this.apiUrl);
  }

  getUser(id: string): Observable<any> {
    return this.http.get(`${this.apiUrl}/${id}`);
  }

  createUser(data: any): Observable<any> {
    return this.http.post(this.apiUrl, data);
  }

  updateUser(id: string, data: any): Observable<any> {
    return this.http.put(`${this.apiUrl}/${id}`, data);
  }

  deleteUser(id: string): Observable<any> {
    return this.http.delete(`${this.apiUrl}/${id}`);
  }
}
</pre>

---

# 🧩 6. CREAR COMPONENTE USERS

ng generate component users  

---

# 🧠 7. COMPONENTE ANGULAR (CRUD LOGIC)

## src/app/users/users.ts

<pre>
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { UserService } from '../services/user';

@Component({
  selector: 'app-users',
  standalone: true,
  imports: [CommonModule, FormsModule],
  templateUrl: './users.html'
})
export class Users {

  users: any[] = [];

  user = {
    name: '',
    email: '',
    age: 0
  };

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.loadUsers();
  }

  loadUsers() {
    this.userService.getUsers().subscribe(data => {
      this.users = data;
    });
  }

  createUser() {
    this.userService.createUser(this.user).subscribe(() => {
      this.loadUsers();
      this.user = { name: '', email: '', age: 0 };
    });
  }

  deleteUser(id: string) {
    this.userService.deleteUser(id).subscribe(() => {
      this.loadUsers();
    });
  }
}
</pre>

---

# 🌍 8. CONFIGURAR RUTAS ANGULAR

## app.routes.ts

<pre>
import { Routes } from '@angular/router';
import { Users } from './users/users';

export const routes: Routes = [
  {
    path: 'users',
    component: Users
  },
  {
    path: '',
    redirectTo: 'users',
    pathMatch: 'full'
  },
  {
    path: '**',
    redirectTo: 'users'
  }
];
</pre>

---

# 🔥 9. BACKEND EXPRESS (CORS HABILITADO)

backend/index.js

<pre>
const cors = require('cors');
app.use(cors());
</pre>

---

# 🚀 10. RESULTADO FINAL

Frontend:
http://IP_VPS:4200/users  

Backend:
http://IP_VPS:3000/api/users  

---

# 🧠 FLUJO COMPLETO

Angular (4200)
   ↓ HTTP
Express API (3000)
   ↓
MongoDB (27017)

---

# 🔥 SISTEMA FINAL

✔ PM2 mantiene backend activo  
✔ Angular consume API REST  
✔ CRUD completo funcionando  
✔ VPS listo para producción  
