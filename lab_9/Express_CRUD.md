# 🚀 API REST con Node.js + Express + MongoDB (Mongoose)

Este proyecto implementa una API REST completa usando Node.js, Express y MongoDB bajo arquitectura MVC.

# 📚 Tecnologías

Node.js, Express, MongoDB, Mongoose, dotenv, CORS

# 🧱 Arquitectura

<pre>
Cliente → Express → Controllers → Models → MongoDB
</pre>

# 📁 Estructura del proyecto

<pre>
api-express/
│
├── index.js
├── .env
├── config/db.js
├── models/User.js
├── routes/userRoutes.js
├── controllers/userController.js
</pre>

# 📦 Instalación

* mkdir backend
* cd backend
* npm init -y
* npm install express mongoose cors dotenv

# ⚙️ .env

PORT=3003
MONGO_URI=mongodb://rllerena:Tecsup123@localhost:27017/mi_api?authSource=admin

# 🚀 index.js

<pre>
const express = require('express');
const cors = require('cors');
const connectDB = require('./config/db');

const app = express();

app.use(cors());
app.use(express.json());

connectDB();

app.use('/api/users', require('./routes/userRoutes'));

app.get('/', (req, res) => {
    res.json({ message: "API funcionando 🚀" });
});

const PORT = process.env.PORT || 3003;

app.listen(PORT, '0.0.0.0', () => {
    console.log("Servidor en http://0.0.0.0:" + PORT);
});
</pre>

# 🗄️ config/db.js

<pre>
const mongoose = require('mongoose');

const connectDB = async () => {
    try {
        await mongoose.connect(process.env.MONGO_URI);
        console.log("MongoDB conectado correctamente");
    } catch (error) {
        console.log("Error MongoDB", error);
        process.exit(1);
    }
};

module.exports = connectDB;
</pre>

# 👤 models/User.js

<pre>
const mongoose = require('mongoose');

const UserSchema = new mongoose.Schema({
    name: String,
    email: String,
    age: Number
});

module.exports = mongoose.model('User', UserSchema);
</pre>

# 🌐 routes/userRoutes.js

<pre>
const express = require('express');
const router = express.Router();
const controller = require('../controllers/userController');

router.post('/', controller.createUser);
router.get('/', controller.getUsers);
router.get('/:id', controller.getUser);
router.put('/:id', controller.updateUser);
router.delete('/:id', controller.deleteUser);

module.exports = router;
</pre>

# 🎮 controllers/userController.js

<pre>
const User = require('../models/User');

exports.createUser = async (req, res) => {
    const user = await User.create(req.body);
    res.json(user);
};

exports.getUsers = async (req, res) => {
    res.json(await User.find());
};

exports.getUser = async (req, res) => {
    res.json(await User.findById(req.params.id));
};

exports.updateUser = async (req, res) => {
    res.json(await User.findByIdAndUpdate(req.params.id, req.body, { new: true }));
};

exports.deleteUser = async (req, res) => {
    await User.findByIdAndDelete(req.params.id);
    res.json({ message: "Usuario eliminado" });
};
</pre>

# 🔌 ENDPOINTS

POST /api/users  
GET /api/users  
GET /api/users/:id  
PUT /api/users/:id  
DELETE /api/users/:id  

# 🚀 EJECUCIÓN

node index.js

npm install -g nodemon
nodemon index.js

# 🌍 ACCESO

http://IP_DE_TU_VPS:3003/

# 🔥 RESULTADO

✔ API REST  
✔ CRUD completo  
✔ MongoDB integrado  
✔ Arquitectura MVC  
✔ Listo para VPS
