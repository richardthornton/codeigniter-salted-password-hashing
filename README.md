# CodeIgniter Salted Password Hashing Library
## Salted Password Hashing - Doing it Right

### Introduction

> "If you're a web developer, you've probably had to make a user account system. The most important aspect of a user account system is how user passwords are protected. User account databases are hacked frequently, so you absolutely must do something to protect your users' passwords if your website is ever breached. The best way to protect passwords is to employ **salted password hashing**."

This library is a direct port from the Salted Password Hashing article at [CrackStation](http://crackstation.net/hashing-security.htm). If you only read ONE page about password security, this is a great one. It has an amazing amount detail that's kind of easy to digest. Seriously, READ IT!

I wanted to build my own CI authentication libraries, but this looked too good to pass up!

### Using with CodeIgniter

1. Add the file Password.php to your /application/libraries folder
2. Load the library when you need to work with a password (probably don't need to autoload): `$this->load->library('password');`

### Saving a password
#### Create the hash and save to the DB

This code is a pretty quick mockup - you'd want to be checking the data was submitted correctly first.

```
$hash = $this->password->create_hash($_POST['pwd']);

$data = array
(
	'name' => $name,
	'email' => $email,
	'password' => $hash
);

$this->db->insert('users', $data);
```

### Validating a login
#### Grab the saved password and check it's valid

```
$this->db->select('password');
$this->db->where('email', $_POST['email']);
$query = $this->db->get('users');
$row = $query->row();

if ($this->password->validate_password($_POST['pwd'], $row->password))
{
	// sweet, everything looks good
	// set up sessions etc etc...
}
else
{
	// doh! send them back
	// only tell the user the "username or password was incorrect"
	// you don't want them knowing they've found a user account!
}
```

### Comments

I doubt this is the perfect way of handing this; I've never saved raw passwords but I'm guilty of the good old `md5()` basics... This will certainly be much more secure!

I hope someone finds this useful. After years of bludging from the work of others I'm glad to give something back, even if it's pretty small. ;)
