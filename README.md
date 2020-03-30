#!/usr/bin/python
# -*- coding: utf-8 -*-

#Lien du site du programme: http://informathix.tuxfamily.org/?q=node/119
 
 
def affiche_peres(pere,depart,extremite,trajet):

    if extremite == depart:
        return [depart] + trajet
    else:
        return (affiche_peres(pere, depart, pere[extremite], [extremite] + trajet))
 
def plus_court(graphe,etape,fin,visites,dist,pere,depart):

    # si on arrive à la fin, on affiche la distance et les peres
    if etape == fin:
       return dist[fin], affiche_peres(pere,depart,fin,[])
    # si c'est la première visite, c'est que l'étape actuelle est le départ : on met dist[etape] à 0
    if  len(visites) == 0 : dist[etape]=0
    # on commence à tester les voisins non visités
    for voisin in graphe[etape]:
        if voisin not in visites:
            # la distance est soit la distance calculée précédemment soit l'infini
            dist_voisin = dist.get(voisin,float('inf'))
            # on calcule la nouvelle distance calculée en passant par l'étape
            candidat_dist = dist[etape] + graphe[etape][voisin]
            # on effectue les changements si cela donne un chemin plus court
            if candidat_dist < dist_voisin:
                dist[voisin] = candidat_dist
                pere[voisin] = etape
    # on a regardé tous les voisins : le noeud entier est visité
    visites.append(etape)
    # on cherche le sommet *non visité* le plus proche du départ
    non_visites = dict((s, dist.get(s,float('inf'))) for s in graphe if s not in visites)
    noeud_plus_proche = min(non_visites, key = non_visites.get)
    # on applique récursivement en prenant comme nouvelle étape le sommet le plus proche 
    return plus_court(graphe,noeud_plus_proche,fin,visites,dist,pere,depart)
 
def dij_rec(graphe,debut,fin):
    return plus_court(graphe,debut,fin,[],{},{},debut)
 
if __name__ == "__main__":
    g3 = {'a': {'d': 2, 'g': 2},
          'b': {'a' : 1}, 
          'c': {'b' : 1, 'f' : 2, 'g' : 3},
          'd': {'g': 4, 's': 7},
          'e': {'a': 5, 'b' : 3, 'c': 2},
          'f': {'d' : 1,'s' : 6},
          'g': {'f' :4},
          's':{}
    }
    l3,c3 = dij_rec(g3,'e','s')
    print ('Plus court chemin ex3 : ',c3, ' de longueur :',l3)
    g4 = {'a': {'d': 5, 'e': 7, 'b' : 2},
          'b': {'e' : 4, 'c' : 9},
          'c': {'e' : 4, 'g' : 6},
          'd': {'e': 3, 'f': 5},
          'e': {'f': 3, 'g' : 4},
          'f': {'h' : 5},
          'g': {'h' : 3},
          'h': {}
    }
    l4,c4 = dij_rec(g4,'a','h')
    print ('Plus court chemin ex4 : ',c4, ' de longueur :',l4)
 
 
 

