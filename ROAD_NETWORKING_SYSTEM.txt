from tkinter import *
import folium
graph = {'a':{'b':569,'c':1890,'d':2175,'e':1448,'f':732,'g':1938,'h':347,'i':2995,'j':1516},
         'b':{'a':569,'c':1492,'d':1584,'e':857,'f':1295,'g':1347,'h':626,'i':2404,'j':1229},
         'c':{'a':1890,'b':1492,'d':1517,'e':1380,'f':2440,'g':975,'h':1678,'i':2336,'j':2033},
         'd':{'a':2175,'b':1584,'c':1517,'e':782,'f':2901,'g':553,'h':2208,'i':811,'j':931},
         'e':{'a':1448,'b':857,'c':1380,'d':782,'f':2168,'g':647,'h':1475,'i':1587,'j':2168},
         'f':{'a':732,'b':1295,'c':2440,'d':2901,'e':2168,'g':2664,'h':774,'i':3721,'j':2254},
         'g':{'a':1938,'b':1347,'c':975,'d':553,'e':647,'f':2664,'h':1963,'i':1372,'j':1233},
         'h':{'a':347,'b':626,'c':1678,'d':2208,'e':1475,'f':774,'g':1963,'i':3027,'j':1869},
          'i':{'a':2995,'b':2404,'c':2336,'d':811,'e':1587,'f':3721,'g':1372,'h':3027,'j':1635},
          'j':{'a':1516,'b':1229,'c':2033,'d':931,'e':2168,'f':2254,'g':1233,'h':1869,'i':1635}
        }
root = Tk()

root.title("Road Networking System")
root.geometry("500x300")
root.configure(background = 'light blue')
large_font = ('Verdana',10)
start_label = Label(root, text="Enter the source(a-j): ",font=large_font)
start_field = Entry(root,font=large_font,width=25)
start_field.grid(row=0, column=1)
start_label.grid(row=0, sticky=E)

goal_label = Label(root, text="Enter the destination(a-j): ",font=large_font)
goal_field = Entry(root,font=large_font,width=25)
goal_field.grid(row=2, column=1)
goal_label.grid(row=2, sticky=E)


blank =Entry(root,font=large_font,width=25)
blank.grid(row=6, column=1)



def makeMap(start,goal):
    my_map = folium.Map(location = [28.5011226, 77.4099794], zoom_start =5)
    # Make an empty map
    folium.Marker([12.9716,77.5946],popup = 'a').add_to(my_map)#Bangalore
    folium.Marker([17.3850,78.4867],popup = 'b').add_to(my_map)#Hyderabad
    folium.Marker([22.5726,88.3639],popup = 'c').add_to(my_map)#Kolkata
    folium.Marker([28.7041,77.1025],popup = 'd').add_to(my_map)#Delhi
    folium.Marker([23.2599,77.4126],popup = 'e').add_to(my_map)#Bhopal
    folium.Marker([08.5241,76.9366],popup = 'f').add_to(my_map)#Trivandrum
    folium.Marker([26.8467,80.9462],popup = 'g').add_to(my_map)#Lucknow
    folium.Marker([13.0827,80.2707],popup = 'h').add_to(my_map)#Chennai
    folium.Marker([34.0837,74.7973],popup = 'i').add_to(my_map)#Sri nagar
    folium.Marker([23.2156,72.6369],popup = 'j').add_to(my_map)#Gandhi nagar
    path=[]
    def dijkstra(start,goal):
        start = str(Entry.get(start_field))
        goal  = str(Entry.get(goal_field))
        shortest_distance = {}
        predecessor = {}
        unseenNodes = graph
        infinity = 9999999
       
        for node in unseenNodes:
            shortest_distance[node] = infinity
        shortest_distance[start] = 0
     
        while unseenNodes:
            minNode = None
            for node in unseenNodes:
                if minNode is None:
                    minNode = node
                elif shortest_distance[node] < shortest_distance[minNode]:
                    minNode = node
     
            for childNode, weight in graph[minNode].items():
                if weight + shortest_distance[minNode] < shortest_distance[childNode]:
                    shortest_distance[childNode] = weight + shortest_distance[minNode]
                    predecessor[childNode] = minNode
            unseenNodes.pop(minNode)
     
        currentNode = goal
        while currentNode != start:
            try:
                path.insert(0,currentNode)
                currentNode = predecessor[currentNode]
            except KeyError:
                Ans=('Path not reachable')
                blank.insert(0,Ans)
                break
        path.insert(0,start)
        if shortest_distance[goal] != infinity:
            Ans = ( "The shortest path is :"+ str(shortest_distance[goal]))
            blank.insert(0, Ans)
          
    dijkstra(start,goal)
    points={'a':(12.9716,77.5946),'b':(17.3850,78.4867),'c':(22.5726,88.3639),'d':(28.7041,77.1025),'e':(23.2599,77.4126),'f':(8.5241,76.9366),
                   'g':(26.8467,80.9462),'h':(13.0827,80.2707),'i':(34.0837,74.7973),'j':(23.2156,72.6369)}


    dictfilt = lambda x, y: dict([ (i,x[i]) for i in x if i in set(y) ])

    result = dictfilt(points,path)

    fresult=list(result.values())


    folium.PolyLine(fresult, color="red", weight=2.5, opacity=1).add_to(my_map)

    my_map.save(" my_map.html ")
    
def show_result():
    start = str(Entry.get(start_field))
    goal  = str(Entry.get(goal_field))
    
    makeMap(start,goal)
    

button1 = Button(root, text="Quit", command=root.destroy,font=large_font)
button1.grid(row=5, column=0)
button2=Button(root, text="Show Result", command=show_result,font=large_font)
button2.grid(row=5 ,column=1)
mainloop()
