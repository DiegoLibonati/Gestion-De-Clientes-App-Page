# Gestion-De-Clientes-App-Page

## Getting Started

### For REACT JS

1. Clone the repository
2. Join to the correct path of the clone
3. Install node_modules with npm install
4. Use npm start to run the app page

### For Flask

1. Join to the correct path of the clone
2. Run the server with python server.py

## Description

I made a web application and its rest api to manage customers. Through different endpoints we will bring different information to be able to show it in front. In this web application we will be able to add, edit and delete customers. To save all the information use Mysql with Phpmyadmin.

## Technologies used

1. REACT JS
2. FLASK
3. PYTHON
4. CSS
5. XAMPP
6. MYSQL
7. SQL
8. PHPMYADMIN

## Galery

![Gestion-de-clientes-App-Page](https://raw.githubusercontent.com/DiegoLibonati/DiegoLibonatiWeb/main/data/projects/Flask/Imagenes/gestionclientesflask-0.jpg)

![Gestion-de-clientes-App-Page](https://raw.githubusercontent.com/DiegoLibonati/DiegoLibonatiWeb/main/data/projects/Flask/Imagenes/gestionclientesflask-1.jpg)

![Gestion-de-clientes-App-Page](https://raw.githubusercontent.com/DiegoLibonati/DiegoLibonatiWeb/main/data/projects/Flask/Imagenes/gestionclientesflask-2.jpg)

## Portfolio Link

`https://diegolibonati.github.io/DiegoLibonatiWeb/#/projects?q=Gestion%20De%20Clientes%20App%20Page`

## Video

https://user-images.githubusercontent.com/99032604/199856418-599b8ed5-802a-42fb-acdd-52d7a7da0249.mp4

## Documentation

### Flask - flask-server/server.py

`getAllCustomers()` is an endpoint that will allow us to fetch all the information of all customers:

```
@app.route('/api/customers')
def getAllCustomers():
    cur = mysql.connection.cursor()
    cur.execute('SELECT * FROM customers')
    data = cur.fetchall()
    result = []
    for row in data:
        content = {
        'id':row[0],
        'firstname': row[1],
        'lastname': row[2],
        'email': row[3],
        'phone': row[4],
        'address': row[5],
         }
        result.append(content)

    return jsonify(result)
```

`saveCustomer()` is an endpoint that will allow us to save a Customer:

```
@app.route('/api/customers', methods=['POST'])
@cross_origin()
def saveCustomer():
    cur = mysql.connection.cursor()
    consulta = cur.execute('SELECT * FROM customers WHERE email LIKE %s', [request.json['email']])

    if consulta == 0:
        cur.execute("INSERT INTO `customers` (`id`, `firstname`, `lastname`, `email`, `phone`, `address`) VALUES (NULL, %s, %s, %s, %s,%s );", (request.json['firstname'],request.json['lastname'],request.json['email'],request.json['phone'],request.json['address']))
    else:
        return abort(400)

    cur.connection.commit()
    return 'Cliente guardado'
```

`removeCustomer()` is an endpoint that will allow us to remove a Customer:

```
@app.route('/api/customers/<int:id_customer>', methods=['DELETE'])
@cross_origin()
def removeCustomer(id_customer):
    cur = mysql.connection.cursor()
    cur.execute(f"DELETE FROM `customers` WHERE `customers`.`id` = {str(id_customer)};")
    cur.connection.commit()
    return 'Cliente eliminado'
```

`editCustomer()` is an endpoint that will allow us to edit a Customer:

```
@app.route('/api/customers', methods=['PUT'])
@cross_origin()
def editCustomer():
    cur = mysql.connection.cursor()
    cur.execute('UPDATE `customers` SET `firstname`=%s, `lastname`=%s, `email`=%s, `phone`=%s, `address`=%s WHERE `customers`.`id` = %s', (request.json['firstname'],request.json['lastname'],request.json['email'],request.json['phone'],request.json['address'], request.json['id']))
    cur.connection.commit()
    return 'Cliente guardado y editado'
```

### React JS

### hooks/useFetchCustomers.js

The CustomHook called `useFetchCustomers()` will allow us to fetch all the information and store it in a `customers` state:

```
export const useFetchCustomers = (newCustomers) => {
  const [customers, setCustomers] = useState([]);

  const getCustomers = async () => {
    const customers = await getAllCustomers();
    setCustomers(customers);
  };

  useEffect(() => {
    getCustomers();
  }, [newCustomers]);

  return {
    customers,
  };
};
```

### helpers/getAllCustomers.js

In this function `getAllCustomers()` we are going to obtain all the customers through a call to the API that we generated with FLASK:

```
export const getAllCustomers = async () => {
  const URL = "http://localhost:5000/api/customers";

  const resp = await fetch(URL, {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
    },
  });

  const customers = await resp.json();

  const customer = customers.map((customer) => ({
    id: customer.id,
    address: customer.address,
    email: customer.email,
    firstname: customer.firstname,
    lastname: customer.lastname,
    phone: customer.phone,
  }));

  return customer;
};
```
