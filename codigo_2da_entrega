import pandas as pd
import gmplot 

df=pd.read_csv('D:\python\calles_de_medellin_con_acoso.csv',sep=';')
dfFiltrado = df.fillna({"harassmentRisk":df['harassmentRisk'].mean()})

grafo={}
for line in dfFiltrado.index:
    temp=((dfFiltrado["harassmentRisk"][line]+dfFiltrado["length"][line])/2,dfFiltrado["length"][line],dfFiltrado["harassmentRisk"])
    if dfFiltrado["origin"][line] not in grafo:
        grafo[dfFiltrado["origin"][line]]={dfFiltrado["destination"][line]:temp}
    else:
        grafo[dfFiltrado["origin"][line]][dfFiltrado["destination"][line]]=temp
        
    if  dfFiltrado["oneway"][line] == True: 
      if dfFiltrado["destination"][line] not in grafo:
        grafo[dfFiltrado["destination"][line]]={dfFiltrado["origin"][line]:temp}
      else:
        grafo[dfFiltrado["destination"][line]][dfFiltrado["origin"][line]]=temp
        
grafoTemp={}
for i in grafo:
    grafoTemp[i]=grafo[i]
    for valor in grafo[i]:
            grafoTemp[i][valor]=grafo[i][valor][0]
 
def dijkstra(inicial,final):
    camino_mas_Corto={}
    camino_Recorrido={}
    nodos_Nousados=grafoTemp
    infinito=9999999
    ruta=[] 
    
    for node in nodos_Nousados:
        camino_mas_Corto[node]=infinito
    camino_mas_Corto[inicial]=0

    while nodos_Nousados:
        distancia_Minima=None
        for node in nodos_Nousados:
            if distancia_Minima is None:
                distancia_Minima=node
            if camino_mas_Corto[node] < camino_mas_Corto[distancia_Minima]:
                distancia_Minima=node
        opciones_Caminos=grafoTemp[distancia_Minima].items()
    
        for tempNodo, indice in opciones_Caminos:
            if indice + camino_mas_Corto[distancia_Minima] < camino_mas_Corto[tempNodo]:
                camino_mas_Corto[tempNodo]=indice+camino_mas_Corto[distancia_Minima]
                camino_Recorrido[tempNodo]=distancia_Minima
        nodos_Nousados.pop(distancia_Minima)   
                  
    nodo_actual=final
    while nodo_actual != inicial:
        try:
            ruta.insert(0,nodo_actual)
            nodo_actual=camino_Recorrido[nodo_actual]
        except KeyError:
            print("La ruta no es valida")
            break   
    ruta.insert(0,inicial)
    if camino_mas_Corto[final] != infinito:
        return  ruta
 
temp=dijkstra('aca va el origen','aca va el destino')

ubicaciones={}

latitude_list = []
longitude_list = [] 
    
for i in range (0,len(temp)):
    valores=str(temp[i])
    ubicaciones[float(valores[1:valores.find(",")])]=float(valores[valores.find(",")+2:len(valores)-1])
    
for i in range (0,len(temp)):
    valores=str(temp[i])
    longitude_list.append(float(valores[1:valores.find(",")]))
    latitude_list.append(float(valores[valores.find(",")+2:len(valores)-1]))
   
gmap3 = gmplot.GoogleMapPlotter(latitude_list[0],longitude_list[0], 13) 
gmap3.scatter( latitude_list, longitude_list, '# FF000', size = 40, marker = False ) 
gmap3.plot(latitude_list, longitude_list, 'cornflowerblue', edge_width = 2.5) 
gmap3.draw( "D:\python\map.html" )
