# Coupling & Cohesion

## Coesão 

- Fazer uma única coisa  
- Uma única reponsabilidade  

"Uma classe coesa é aquela que possui uma única responsabilidade.  
Ou seja, ela não toma conta de mais de um conceito no sistema.  
Se a classe é responsável por representar uma Nota Fiscal, ela representa apenas isso.  
As responsabilidades de uma Fatura, por exemplo, estarão em
outra classe."   
***(Mauricio Aniche, Orietançao a Objetos e SOLID para Ninjas)***



```python

from pydantic import BaseModel
import json

class Pedido(BaseModel):
    id: int = 0
    codigo_produto: str
    quantidade: int
    codigo_cliente: int


PEDIDOS_PATH = '/pedidos/pedidos.json'
''' file content:
[
    {"id": 1, "codigo_produto": "monitor_22_pol_samsung", "quantidade": 1, "codigo_cliente": "55448899"},
]
'''


@app.post("/pedidos/", response_model=Pedido)
def criar_pedido(pedido: Pedido):
    
    with open(PEDIDOS_PATH, 'w') as _pedidos_fp:
        pedidos = json.load(_pedidos_fp)

        ids = [p.id for o in pedidos]
        new_id = max(ids) + 1
        pedido.id = new_id
        pedidos.append(pedido)
        json.dump(pedidos, _pedido_fp)

    return pedido

@app.put("/pedidos/{id}", response_model=Pedido)
def atualizar_pedido(id:int, pedido: Pedido):
    
    if len(pedido.codigo_produto) < 10:
        raise Exception("Codigo do Produto invalido")

    with open(PEDIDOS_PATH, 'w') as _pedidos_fp:
        pedidos = json.load(_pedidos_fp)

        pedidos_dict = { p['id']:p for p in pedidos}

        pedidos_dict[pedido.id] = pedido
        json.dump(list(pedidos_dict.values()), _pedido_fp)

    return pedido


```



```python

from pydantic import BaseModel
import json

class Pedido(BaseModel):
    id: int = 0
    codigo_produto: str
    quantidade: int
    codigo_cliente: int


@app.post("/pedidos/", response_model=Pedido)
def criar_pedido(pedido: Pedido):
    
    pedidoDao = PedidoDAO()
    pedido = pedidoDao.inserir(pedido)
    return pedido


@app.put("/pedidos/{id}", response_model=Pedido)
def atualizar_pedido(id:int, pedido: Pedido):
    
    pedidoDao = PedidoDAO()
    _pedido = pedidoDao.get(id)

    pedidoDao.atualizar(id, pedido)

    return pedido


```


## Acoplamento

"In software engineering, coupling is the degree of interdependence between software modules;   
a measure of how closely connected two routines or modules are;[1]    
the strength of the relationships between modules.[2]"   
(wikipedia)

O grau de dependencia entre 2 ou mais partes.


``` python

PEDIDOS_PATH = '/pedidos/pedidos.json'

class PedidoDAOInterface(@abc)
    def __init__(self, config: dict)
        pass

    def inserir(pedido: Pedido)
        raise NotImplementedException()


class PedidoDAO(PedidoDAOInterface)
    
    def inserir(pedido: Pedido):
        with open(PEDIDOS_PATH, 'w') as _pedidos_fp:
            pedidos = json.load(_pedidos_fp)

            ids = [p.id for o in pedidos]
            new_id = max(ids) + 1
            pedido.id = new_id
            pedidos.append(pedido)
            json.dump(pedidos, _pedido_fp)

    return pedido

class PedidoDAO2(PedidoDAOInterface)
    
    def inserir(pedido: Pedido):
        #    persiste mongo

    return pedido


class Factory:
    @static
    def make_pedidoDAO():
        return PedidoDAO({})


@app.post("/pedidos/", response_model=Pedido)
def criar_pedido(pedido: Pedido):
    
    pedidoDao = Factory.make_pedidoDAO()

    pedido = pedidoDao.inserir(pedido)
    return pedido


```

![Usb](./images/usb.jpeg)




