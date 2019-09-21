Cette librairie est surement inutile à part pour moi :=)

Je l'utilise pour mes scripts de migration.

Elle est bien moins fini que plusieurs autres que l'on peut trouver sur le web

exemple:

```
# -*- encoding: utf-8 -*-
from openerp_connection import Openerp, Openerp_db, Module

db = "demo"
protocole = 'http://'
serveur = '127.0.0.1'
port = '8069'
PASS_ADMIN_OPENERP = 'admin'
user = 'admin'
password = 'admin'

"""Creation de base"""
connect_db = Openerp_db(protocole, serveur, port)
if db not in connect_db.list():
    connect_db.sock.create_database(PASS_ADMIN_OPENERP, db, False, 'fr_FR', password)

""" Connection """
connection = Openerp('http://', '127.0.0.1', '8069', 'demo', 'admin', 'admin')

if connection.uid:
    """ Recherche Id"""
    partenaires = connection.search('res.partner', [])

    """ Lecture """
    for partner_id in partenaires:
        print(connection.read('res.partner', partner_id))

    """ Recherche et lecture"""
    partenaires = connection.search_read('res.partner', [])
    for partenaire in partenaires:
        print(partenaire)

    """ Creation """
    partner_create_id = connection.create('res.partner', {'name': 'Eric Vernichon'})

    """ ecriture """
    connection.write('res.partner',partner_create_id,{'email': 'Vernichon Eric <eric@vernichon.ch>'})

    """ Execute """
    print(connection.execute('res.partner', 'update_address', [partner_create_id],{'street':'Rue de la pompe'}))
    print(connection.execute('res.partner', 'name_get', partner_create_id))
    print(connection.read('res.partner',partner_create_id,fields=['street','email']))
    """ Supression """
    connection.unlink('res.partner', partner_create_id)

    """ Mise a jour liste des modules """
    connect_module = Module(connection)

    connect_module.update_list()

    """ Install Module """
    connect_module.install('project')

    """ Update Module """
    connect_module.update('project', force=True)

    """Supresssion de base"""
    connect_db.sock.drop(PASS_ADMIN_OPENERP, db)
else:
    print("Erreur de connection")

```
