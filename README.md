# KB-TUGAS-4
| Nama                      | NRP           |Username           |
|---------------------------|---------------|--------------|
|Moh rosy haqqy aminy       |5025211012     |hqlco          |
|M. hafidh Rosyadi          |5025211013     |Hfdrsyd             |
|Hammuda Arsyad             |5025211146     |H-mD         |

--------

## code
```bash
import networkx as nx
import matplotlib.pyplot as plt
# from pyvis.network import Network
# from IPython.core.display import display, HTML

G = nx.Graph()

G.add_node("Magetan", h=162, visited=False, dis=None, parent=None)
G.add_node("Surabaya", h=0, visited=False, dis=None, parent=None)
G.add_node("Ngawi", h=130, visited=False, dis=None, parent=None)
G.add_node("Ponorogo", h=128, visited=False, dis=None, parent=None)
G.add_node("Madiun", h=126, visited=False, dis=None, parent=None)
G.add_node("Bojonegoro", h=60, visited=False, dis=None, parent=None)
G.add_node("Nganjuk", h=70, visited=False, dis=None, parent=None)
G.add_node("Jombang", h=36, visited=False, dis=None, parent=None)
G.add_node("Lamongan", h=36, visited=False, dis=None, parent=None)
G.add_node("Gresik", h=12, visited=False, dis=None, parent=None)
G.add_node("Sidoarjo", h=22, visited=False, dis=None, parent=None)
G.add_node("Probolinggo", h=70, visited=False, dis=None, parent=None)
G.add_node("Situbondo", h=146, visited=False, dis=None, parent=None)
G.add_node("Bangkalan", h=140, visited=False, dis=None, parent=None)
G.add_node("Sampang", h=90, visited=False, dis=None, parent=None)
G.add_node("Pamekasan", h=104, visited=False, dis=None, parent=None)
G.add_node("Sumenep", h=150, visited=False, dis=None, parent=None)

G.add_edge("Magetan", "Ngawi", label="32")
G.add_edge("Magetan", "Madiun", label="22")
G.add_edge("Magetan", "Ponorogo", label="34")
G.add_edge("Ngawi", "Bojonegoro", label="44")
G.add_edge("Ngawi", "Madiun", label="30")
G.add_edge("Ponorogo", "Madiun", label="29")
G.add_edge("Madiun", "Nganjuk", label="40")
G.add_edge("Bojonegoro", "Lamongan", label="42")
G.add_edge("Bojonegoro", "Jombang", label="70")
G.add_edge("Bojonegoro", "Nganjuk", label="33")
G.add_edge("Nganjuk", "Jombang", label="40")
G.add_edge("Jombang", "Surabaya", label="72")
G.add_edge("Lamongan", "Gresik", label="14")
G.add_edge("Gresik", "Surabaya", label="12")
G.add_edge("Surabaya", "Bangkalan", label="44")
G.add_edge("Surabaya", "Sidoarjo", label="25")
G.add_edge("Bangkalan", "Sampang", label="52")
G.add_edge("Sidoarjo", "Probolinggo", label="78")
G.add_edge("Sampang", "Pamekasan", label="31")
G.add_edge("Probolinggo", "Situbondo", label="99")
G.add_edge("Pamekasan", "Sumenep", label="54")


def getData():
    for n, h, _, _, _ in G.nodes.data():
        print(n, "=", h)
    print()
    for n, nbr, w in G.edges.data():
        print(n, nbr, w)


def greedy(start, end):
    n = start
    path = [start]
    while n != end:
        min = None

        # # uncomment to debug
        # print(" ", n, ":")

        for nbr in G.adj[n]:
            if min is None:
                min = nbr
            elif G.nodes[min]['h'] > G.nodes[nbr]['h']:
                min = nbr

            # # uncomment to debug
            # print("   ", min, "=", G.nodes[min]['h'])

        n = min
        path.append(min)
    return path


def astar(start, end):
    open = [start]
    G.nodes[start]['dis'] = 0
    G.nodes[start]['parent'] = start

    while len(open) > 0:
        min = None

        for n in open:
            if min is None or (G.nodes[min]['h'] + G.nodes[min]['dis']) > (G.nodes[n]['h'] + G.nodes[n]['dis']):
                min = n

        if min is end:
            path = []
            n = min
            while G.nodes[n]['parent'] is not n:
                path.append(n)
                n = G.nodes[n]['parent']

            path.append(start)
            path.reverse()

            return path

        for n in G.adj[min]:
            distance = G.nodes[min]['dis'] + int(G.edges[min, n]['label'])

            if n not in open and not G.nodes[n]['visited']:
                open.append(n)
                G.nodes[n]['dis'] = distance
                G.nodes[n]['parent'] = min

            elif G.nodes[n]['dis'] > distance:
                G.nodes[n]['dis'] = distance
                G.nodes[n]['parent'] = min

                if G.nodes[n]['visited']:
                    G.nodes[n]['visited'] = False
                    open.append(n)

        open.remove(min)
        G.nodes[min]['visited'] = True

    return None


# # uncomment to debug
# print("Greedy Process :")
greedy = greedy("Magetan", "Surabaya")
# for i in range(len(greedy)):
#   if i is not len(greedy)-1:
#     G.edges[greedy[i], greedy[i+1]]["color"] = "red"
#     G.edges[greedy[i], greedy[i+1]]["weight"] = 5

print()
print("Greedy Path :")
print(' >> '.join(greedy))
print()

# # uncomment to debug
# print("A* Process :")
astar = astar("Magetan", "Surabaya")
# for i in range(len(astar)):
#   if i is not len(astar)-1:
#     G.edges[astar[i], astar[i+1]]["color"] = "green"
#     G.edges[astar[i], astar[i+1]]["weight"] = 5

print()
print("A* Path :")
print(' >> '.join(astar))
print()

nx.draw(G, with_labels=True)
plt.savefig("graph.png")
# net = Network(notebook=True, cdn_resources='in_line')
# net.from_nx(G)
# net.toggle_physics(True)
# net.show("graph.html")
# display(HTML("graph.html"))

```
