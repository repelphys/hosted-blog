+++
date = '2025-01-05T12:51:39Z'
draft = false
title = 'Server Initialisation (AuthService)'
weight = 20
[params]
  author = 'Abhinav K.'
+++

### Server Initialisation

The foundation is set by establishing two main systems:
	- Firebase Admin SDK
	- Express.js

````
const serviceAccount = JSON.parse(
  fs.readFileSync('./config/firebase-adminsdk.json', 'utf8')
);

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
});

const auth = admin.auth();
const db = admin.firestore();

const app = express();
app.use(express.json());
app.use(cors());
`````
