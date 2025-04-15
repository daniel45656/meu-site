cadastro-api/
├── server.js
├── models/
│   └── User.js
├── routes/
│   └── auth.js
├── controllers/
│   └── authController.js
├── .env
├── package.json
1️⃣ server.js
js
Copiar
Editar
require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const authRoutes = require('./routes/auth');

const app = express();
app.use(express.json());

mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log("✅ MongoDB conectado"))
  .catch(err => console.error("Erro ao conectar no MongoDB:", err));

app.use('/api/auth', authRoutes);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`🚀 Servidor rodando na porta ${PORT}`));
2️⃣ .env
env
Copiar
Editar
MONGO_URI=mongodb+srv://<usuario>:<senha>@seu-cluster.mongodb.net/cadastro
JWT_SECRET=sua_chave_secreta_aqui
Troque <usuario>, <senha> e seu-cluster com os dados do seu MongoDB Atlas

3️⃣ models/User.js
js
Copiar
Editar
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  nome: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  senha: { type: String, required: true }
});

module.exports = mongoose.model('User', userSchema);
4️⃣ routes/auth.js
js
Copiar
Editar
const express = require('express');
const router = express.Router();
const { registerUser, loginUser } = require('../controllers/authController');

router.post('/register', registerUser);
router.post('/login', loginUser);

module.exports = router;
5️⃣ controllers/authController.js
js
Copiar
Editar
const User = require('../models/User');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

exports.registerUser = async (req, res) => {
  const { nome, email, senha } = req.body;
  try {
    const userExist = await User.findOne({ email });
    if (userExist) return res.status(400).json({ msg: "Usuário já existe" });

    const hashedSenha = await bcrypt.hash(senha, 10);
    const novoUser = new User({ nome, email, senha: hashedSenha });

    await novoUser.save();
    res.status(201).json({ msg: "Usuário registrado com sucesso" });
  } catch (err) {
    res.status(500).json({ msg: "Erro no servidor" });
  }
};

exports.loginUser = async (req, res) => {
  const { email, senha } = req.body;
  try {
    const user = await User.findOne({ email });
    if (!user) return res.status(400).json({ msg: "Usuário não encontrado" });

    const isMatch = await bcrypt.compare(senha, user.senha);
    if (!isMatch) return res.status(400).json({ msg: "Senha incorreta" });

    const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET, { expiresIn: '1d' });
    res.json({ token, user: { nome: user.nome, email: user.email } });
  } catch (err) {
    res.status(500).json({ msg: "Erro no login" });
  }
};
6️⃣ package.json (se quiser gerar com npm init -y, depois instale os pacotes)
bash
Copiar
Editar
npm init -y
npm install express mongoose dotenv bcrypt jsonwebtoken
7️⃣ Testando no Insomnia ou Postman:
📥 Cadastro:
POST: http://localhost:5000/api/auth/register

json
Copiar
Editar
{
  "nome": "Maria Oliveira",
  "email": "maria@email.com",
  "senha": "123456"
}
🔐 Login:
POST: http://localhost:5000/api/auth/login

json
Copiar
Editar
{
  "email": "maria@email.com",
  "senha": "123456"
}
