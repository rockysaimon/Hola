# Hola
tengo un dataset que tiene unos registros, pero tiene una columnas llamadas camposonsulta9, campoconsulta11 y así desde la 1 hasta la 33, resulta que sólo necesito 4 de esos campos, y estos los tengo en un excel, donde por cada tipo de monitoreo (es una columna) están los campos que necesito con su nombre, eje, el monitoreo de buena practicas tiene el id en campoconsulta2, el nombre en campoconsulta22 y alertas en campoconsulta4, mientras que el monitoreo de fraude tiene el id en campoconsulta12, el nombre en campoconsulta30 y alertas en campoconsulta40, lo que quiero es mapear esos campos del excel en el pandas, para que el nuevo df, tengo (según el monitoreo) sólo las 4 columnas que necesita, junto a los demás columnas (es decir, de los campoconsulta1 al 30, sólo tener los 3 que necesito y mapeé, pero las demás columnas estén todavía), tengo el siguiente código:
def mapeo_de_campos_bizagi(df_alertas):
    
    LOG.log('Mapeando campos maestros')
    
    df_gestion_rel = pd.read_excel(maestroCampos)

    #Saca el listado de monitoreos sin repetir
    df_monitoreos= df_gestion_rel.drop_duplicates()       
    
    alertas_monitoreo = pd.DataFrame([])
  
    
    #Mapeo del nombre de los campos y consolida en una sola tabla todas las alertas
    for col, fila in df_monitoreos.iterrows(): 
        
        
        temp_alertas_monitoreo = df_alertas.loc[df_alertas['MONITOREO']== str(fila[0])]
        
        sensibilizacion_tmp = fila['SENSIBILIZACION']
        comentarios_tmp = fila['COMENTARIOS'] 
        cod_alerta_tmp = fila['COD_ALERTA']
        
        
        temp_alertas_monitoreo.rename(columns={str(cod_alerta_tmp):'COD_ALERTA',str(sensibilizacion_tmp):'SENSIBILIZACION',str(comentarios_tmp):'COMENTARIOS'}, inplace=True)
        temp_alertas_monitoreo=temp_alertas_monitoreo.reindex(columns=['MONITOREO','COD_ALERTA','SENSIBILIZACION','COMENTARIOS','FECHA_RADICACION','FECHA_CIERRE','COD_BIZAGI','FECHA_ALERTA_JEFE_JEFE','FECHA_ALERTA_JEFE_GH', 'USUARIO_RADICADOR','NOMBRE_RADICADOR','USUARIO_ATENCION','NOMBRE_ATENCION'])
        
        alertas_monitoreo = pd.concat([alertas_monitoreo,temp_alertas_monitoreo],axis=0,ignore_index=True)
    
    
    return alertas_monitoreo   

    donde df_alertas es el df con todos los datos y maestrocampos es la ruta del archivo de excel para el mapeo, al hacerlo así, me sale el error de SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame. Try using .loc[row_indexer,col_indexer] = value instead, cómo lo hago?
