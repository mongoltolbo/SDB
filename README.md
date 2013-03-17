SDB
===

###### Simple DataBase is a simpler IndexedDB Module ######

This module is a vanilla-JavaScript library for manipulating the HTML5 IndexedDB API. Its main goals are simplicity and intuitiveness.

## Quick Start ##


## Overview ##
Hook the API:

	var idb = sdb, PeopleDB = idb.req(PeopleDBschema, function(db){
		// ALL transactions for "PeopleDB" go here!
	});

To create or open an IndexedDB database, you need to have a schema in place. In IndexedDB, you need to have a database name and a version to open/create one. (In IndexedDB, you use the same method to create a new AND open an existing database). Requesting a database will open an existing database or create a new one if it doesn't already exist.

To request a database, use the 'req' method on the API. The 'req' method takes 2 arguments: a database schema, and a callback closure for making transactions.

##### The Schema: #####
The schema is an object-literal. It must consist of a 'db' property (which is a string containing the database's name), and a version (for database verioning). On top of a db name and version, you'll need to set up your "ObjectStores" and Indices. You can add multiple ObjectStores and Indices using arrays. The following elucidates what properties are necessary.

	var PeopleDBschema = {
		db: 'PeopleDB',
		v: 1,
		upgrade: {
			stores: [
				{
					name: 'humans',
					opts: {keyPath: 'id', autoIncrement: true},
					indices: [
						{index: 'name', opts: {unique: false}},
						{index: 'email', opts: {unique: true}}
					]
				},
				{
					name: 'aliens', // aliens are people too!
					opts: {keyPath: 'id', autoIncrement: true},
					indices: [
						{index: 'name', opts: {unique: false}},
						{index: 'email', opts: {unique: true}}
					]
				}
			]
		} 
	};
	
###### Versioning ######
To change your DB-Schema based upon version, use the 'v'-property of the schema-object-literal and make sure to remove any stores (schema.upgrade.stores[i]) from the previous versio(s); then add a new store with the appropriate options and indices:

	var PeopleDBschema = {
		db: 'PeopleDB',
		v: 2,
		upgrade: {
			stores: [
				{
					name: 'cats',  // I'm a cat-lover
					opts: {keyPath: 'id', autoIncrement: true},
					indices: [
						{index: 'name', opts: {unique: false}},
						{index: 'email', opts: {unique: true}}
					]
				},
				{
					name: 'dogs',  // I'm a dog-lover
					opts: {keyPath: 'id', autoIncrement: true},
					indices: [
						{index: 'name', opts: {unique: false}},
						{index: 'email', opts: {unique: true}}
					]
				}
			]
		} 
	};

After creating a new schema-object for this new version, it is recommended that you store previous version-schemas in a README file with a Code-Comment-Note in you code that references the README. (DB-VERSIONING GETS MESSY!!!).

##### The Callback: #####
The callback function will receive the database that was requested as an argument. You'll need this for all of your transactions.

	var idb = sdb, PeopleDB = idb.req(PeopleDBschema, function(db){
		var db = db;
	});















