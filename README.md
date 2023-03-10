# Bank Application Front-end

- npm install to add dependancies
- npm start to run the application

# Authentication

- The app is secured using JWT. When a user creates an account, their data is sent to the server through fetch api using a POST request.In the backend upon receiveing the user data, the password is hashed before being stored in the database. It user the userId generated by MongoDB to create a JWT token to authenticate the user. In response we get a JWT token and the user email. The email and token are both set to the local storage of the client's computer to persist login even when they close the tab. This is purely for good user experience where a user is logged in after creating an account instead of being redirected to the login page.

- The login component also works the same as the signup component, where user details are sent to the server using a POST request. In response we get JWT token and user's email address. The token is used to authenticate the user ald log them in into their account.

- On logout, the user's token in the local storage is deleted. An action type 'LOGOUT' is fired, in state management using React Context where user is set to null. This hence navigates the user to the login page since the account page is a protected route and they can only acces it when logged in.

# Bank Application backend

- npm install
- npm run server to start the server

- NB: Create a .env file in your backend root folder. Add a PORT, MONGO_URI and the SECRET. Make sure you replace the MONGODB_URI in the .env file with yours.

## Database

### MongoDB

- This is a NOSQL database. Data is stored in collections
- Collections used in the database are accounts, users and transactions

### Database Schema

- Accounts: \_id, user_id(FK{user.\_id}), name, email, accountNumber, createdAt, updatedAt
- Users: \_id, name, email, password, createdAt, updatedAt
- Transactions: \_id, user_id(FK{user.\_id}), transactionType, amount, createdAt, updatedAt

- The Accounts and Transactions schema are both related to the user through user_id from the User schema

### Database Abstraction Layer

- Data Abstraction Layer is a connecting point between data from the front-end and data contained in the database. All of the logic is contained in a directory called controllers. There are 3 controllers than handle logic of the user, transactions and accounts. It abstracts the underlying data of MongoDB from the application code. A well defined DAL means that one can use any database of choice without having to change apllication code.

### API Endpoints

- There are 3 main api entry points in the server.js file to the backend server. Under each endpoints are different endpoints.

#### app.use('/api/account', bankRoutes);

- get('/', getAllAccounts) - GET all accounts available in the database
- get('/:id', getAccount) - GET single account by id received from the front-end
- post('/create-account', createAccount) - POST account details on account creation
- patch('/:id', updateBalance) - PATCH update account balance when user makes a deposit or withdrawal

#### app.use('/api/auth', authRoutes);

- post('/login', loginUser) - POST authenticate user on login and sends back token if user is verified
- post('/signup', signupUser) - POST create user account, returns token and account details on succesful registration

#### app.use('/api/transactions', transcationRoutes);

- get('/', getTransactions) - GET all transactions of particular user using id identification
- post('/', transaction) - POST creates new transactions when a user deposits or withdraws money from their account

### API Tools

- For testing of API endpoints I use postman

### API Protocol

- RESTful Api
- Endpoint : The destination URL from which data is being requested.
- Method : We use predefined methods such as GET, POST, PUT or DELETE to fetch the data. These methods vary from one other. Ex. in when using GET, the data is appended to the end of the URL string, whereas in POST, the data is sent along with the HTTP request.
- Headers : They define the request's details and dictate the proper format in which the response must be received. For most requests made a token was added to the headers to authenticate the user.
- body (data) : The actual data sent by the service.

### Additional features

- Bank account number attachment. A random number of 9 digits was attached to the backend and stored to the database on account creation. On the frontend the data is concatenated with 3 zeros to create a 12 digit account number.
- Transactions - A user is able to get all his transactions whether he deposits or withdraws money from their account.
