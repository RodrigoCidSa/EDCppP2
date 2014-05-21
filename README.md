EDCppP2
=======

#include <iostream>

using namespace std;

// declaracion de tipos
typedef struct nodo{
	int elemento;
	nodo *sig;
}tnodo;

typedef nodo *tlista;

// Interfaz para que el main pueda acceder a los métodos = declaracion de funciones
bool esVacia(tlista lista);
void mostrarLista(tlista lista);
void insertar(tlista *lista, int elemento);
void insertarFinal(tlista *lista, int elemento);
void insertarOrdenado(tlista *lista, int elemento);
void eliminarPrimero(tlista *lista);
void eliminarUltimo(tlista *lista);
bool eliminar(tlista *lista, int elemento);
int mayorElemento(tlista lista);
int repeticiones(tlista lista, int elemento);
int mayorYRepeticiones(tlista lista, int elemento);
int sumaRecursiva(tnodo *lista);
bool esOrdenada(tlista lista);
void eliminarOcurrenciasIterativa(tlista *lista, int elemento);
void eliminarOcurrenciasRecursiva(tlista *lista, tnodo *siguiente, tnodo *anterior, int elemento);
void invertirLista(tlista *lista);
void intercambiaNodos(tlista * lista, int e1, int e2);



// MAIN ------------------------------------------------------------------------------------------

int main(){ // programa main de prueba de metodos
	tlista lista;
	lista=NULL;
	if (esVacia(lista))
		cout<<"vacia"<<endl;
	else
		cout<<"no esta vacia"<<endl;
	for(int i=5; i>0; i--){
		cout<<"insertar elemento "<<i<<endl;
		insertar(&lista, i);
	}
	for(int j=6; j<9; j++){
		cout<<"insertarFinal elemento "<<j<<endl;
		insertarFinal(&lista, j);
	}
	cout<<"insertarOrdenado elemento 9"<<endl;
	insertarOrdenado(&lista, 9);
	mostrarLista(lista);
	if(esOrdenada(lista))
		cout<<"La lista esta ordenada"<<endl;
	else
		cout<<"La lista no esta ordenada"<<endl;
	if (esVacia(lista))
			cout<<"vacia"<<endl;
		else
			cout<<"no esta vacia"<<endl;
	eliminarPrimero(&lista);
	eliminarUltimo(&lista);
	eliminar(&lista, 5);
	mostrarLista(lista);
	cout<<"El mayor es "<<mayorElemento(lista)<<" y se repite "<< mayorYRepeticiones(lista, 4) <<" veces"<<endl;
	insertar(&lista, 10);
	insertarFinal(&lista, 10);
	cout<<"El mayor es "<<mayorElemento(lista)<<" y se repite "<< mayorYRepeticiones(lista, 4) <<" veces"<<endl;
	cout<<"La suma de todos los elementos es = "<<sumaRecursiva(lista)<<endl;;
	if(esOrdenada(lista))
		cout<<"La lista esta ordenada"<<endl;
	else
		cout<<"La lista no esta ordenada"<<endl;
	mostrarLista(lista);
	cout<<"Eliminando todas las ocurrendias del numero 10 iterativamente"<<endl;
	eliminarOcurrenciasIterativa(&lista, 10);
	mostrarLista(lista);
	cout<<"Insertamos otra vez el 10 al principio y al final"<<endl;
	insertar(&lista, 10);
	insertarFinal(&lista, 10);
	mostrarLista(lista);
	eliminarOcurrenciasRecursiva(&lista, lista, NULL, 10);
	mostrarLista(lista);
	invertirLista(&lista);
	mostrarLista(lista);
	intercambiaNodos(&lista, 7, 3);
	mostrarLista(lista);
}





// Implementacion de funciones ------------------------------------------------------------------

bool esVacia(tlista lista){
	return (lista == NULL);
}

void mostrarLista(tlista lista){
	tlista ptraux; // Declaramos un puntero auxiliar a nodo
	ptraux=lista; // Le asignamos el valor del primer elemento de la lista (apunta al primer nodo)
	cout<<"Lista = ";
	while (ptraux!=NULL){ // mientras quede lista
		cout<< ptraux->elemento<<", "; // escribimos la info del nodo
		ptraux = ptraux->sig; // ahora hacemos que ptraux apunte al sigiente nodo de la lista
	}
	cout<<endl;
}

void insertar(tlista *lista, int elemento){
	tnodo *nuevonodo;
	nuevonodo=new tnodo;
	nuevonodo->elemento=elemento;
	nuevonodo->sig = (*lista);
	*lista=nuevonodo;
}

void insertarFinal(tlista *lista, int elemento){
	tnodo *nuevonodo;
	tnodo *ptraux;
	nuevonodo=new tnodo;
	nuevonodo->elemento=elemento;
	nuevonodo->sig=NULL; //al ser el ultimo elemento apuntará a NULL
	if (esVacia(*lista)) //si la lista está vacia, lo ponemos a la cabeza de la lista
		*lista=nuevonodo;
	else{
		ptraux=*lista;
		while (ptraux->sig != NULL){ //paramos cuando ptraux apunte al ultimo nodo
			ptraux=ptraux->sig;
		}
		ptraux->sig=nuevonodo; // Hacemos que el ultimo nodo de la lista apunte al nuevonodo
	}
}

void insertarOrdenado(tlista *lista, int elemento){
	tnodo *nuevonodo;
	tnodo *ptraux;
	nuevonodo=new tnodo;
	nuevonodo->elemento=elemento;
	if (esVacia(*lista)){ //si la lista está vacia, lo ponemos a la cabezacde la lista
		nuevonodo->sig=NULL;
		*lista=nuevonodo;;
	}
	else if((*lista)->elemento>elemento) {/* si el primer elemento de la lista ya es mayor que nuestro elemento
								lo insertamos el primero */
		nuevonodo->sig=*lista;
		*lista=nuevonodo;
	}
	else { /*recorremos la lista hasta quedarnos en el ultimo que sea menor que nuestro elemento, es decir
		cuando el siguiente sea mayor o cuando se acabe la lista, entonces lo insertaremos en medio o el ultimo */
		ptraux=*lista;
		while ((ptraux->sig!=NULL)&&(ptraux->sig->elemento<elemento))
			ptraux=ptraux->sig;
		// Ahora lo insertamos despues de ptraux, sea el ultimo o uno intermedio
		nuevonodo->sig=ptraux->sig;
		ptraux->sig=nuevonodo;
	}
}

void eliminarPrimero(tlista *lista){
	tnodo *nodoaeliminar;
	if (! esVacia(*lista)){
		nodoaeliminar=*lista;
		*lista=(*lista)->sig; //ahora lista comienza en lista->sig
		delete(nodoaeliminar);
	}
}

void eliminarUltimo(tlista *lista){
	tnodo *nodoaeliminar;
	tnodo *ptraux;
	if (! esVacia(*lista)){
		if ((*lista)->sig == NULL) //si solo hay un elemento en la lista
			delete(*lista);
		else{
			ptraux=*lista;
			while(ptraux->sig->sig != NULL) //esto quiere decir que el ptraux->sig es el ultimo nodo
				ptraux=ptraux->sig;
			nodoaeliminar=ptraux->sig;
			ptraux->sig=NULL; //modificamos el penultimo para que apunte a null
			delete(nodoaeliminar);
		}
	}
}

bool eliminar(tlista *lista, int elemento){
	/*devuelve verdadero si se elimino el elemento y falso si no estaba
	en la lista */
	tlista nodoaeliminar;
	tlista ptractual, ptranterior;
	bool encontrado=false;
	if (*lista != NULL){
		if ((*lista)->elemento == elemento){ //si elemento está el primero
			encontrado=true;
			nodoaeliminar=*lista;
			*lista=(*lista)->sig;
			delete(nodoaeliminar);
		}
		else{
			ptranterior=*lista;
			ptractual=(*lista)->sig;
			while((ptractual != NULL)&&(ptractual->elemento != elemento)){
				ptranterior=ptractual;
				ptractual=ptractual->sig;
			} //saldremos de este bucle cuando encontremos el elemento o se acabe la lista sin encontralo
			// asi que preguntamos
			if(ptractual != NULL){ // si hemos encontrado el elemento
				encontrado=true;
				nodoaeliminar=ptractual;
				ptranterior->sig=ptractual->sig; //lo saltamos en la lista
				delete(nodoaeliminar);
			}
		}
	}
	return encontrado;
}

int mayorElemento(tlista lista){
	// Complejidad O(n), donde n sera la longitud de la lista, por el bucle while que la recorre
	int elemAux=0;
	if(! esVacia(lista)){
		elemAux=lista->elemento;
		tnodo *ptrAux=lista->sig;
		while(ptrAux != NULL){
			if(elemAux < ptrAux->elemento)
				elemAux=ptrAux->elemento;
			ptrAux=ptrAux->sig;
		}
	}
	return elemAux;
}

int repeticiones(tlista lista, int elemento){
	// Complejidad O(n), donde n es la longitud de la lista
	int rep=0;
	tnodo *ptrAux=lista;
	while(ptrAux != NULL){
		if(ptrAux->elemento == elemento)
			rep++;
		ptrAux=ptrAux->sig;
	}
	return rep;
}

int mayorYRepeticiones(tlista lista, int elemento){
	/* He partido el funcionamiento de la función en otras dos funciones, mayorElemento que busca el mayor,
	 * y repeticiones que busca las veces que se repite un determinado elemnto.
	 *
	 * La complejidad sera O(n), ya que tanto repeticiones() como mayorElemento() recorren cada uno la lista de
	 * principio a fin, por tanto n sera la longitud de la lista
	 */
	return repeticiones(lista, mayorElemento(lista));
}

int sumaRecursiva(tnodo *lista){
	// Complejidad O(n) debido a que recorre la lista entera como si fuera iterativamente, pero de forma recursiva
	int suma=0;
	if(! esVacia(lista)){
		suma=suma+(lista->elemento);
		suma=suma+sumaRecursiva(lista->sig);
	}
	return suma;
}

bool esOrdenada(tlista lista){
	// Complejidad O(n) debido a que recorre la lista de principio a fin, n = longitud de la lista
	bool cond=true;
	int elemAux=0;
	tnodo *ptrAux=lista;
	while((ptrAux != NULL)&&(cond)){
		cond=(elemAux <= ptrAux->elemento);
		elemAux=ptrAux->elemento;
		ptrAux=ptrAux->sig;
	}
	return cond;
}

void eliminarOcurrenciasIterativa(tlista *lista, int elemento){
	/* Complejidad O(n) donde n sera la longitud de la lista que tenga que recorrer para eliminar todas
	 * las ocurrendias del elemento
	 */
	int rep = repeticiones(*lista, elemento);
	for(int i=0; i<rep; i++){
		eliminar(lista, elemento);
	}
}

void eliminarOcurrenciasRecursiva(tlista *lista,tnodo *actual, tnodo *anterior, int elemento){
	// Complejidad O(n), ya que recorre toda la lista como si fuera iterativamente
	if(! esVacia(actual)){
		if(actual->elemento == elemento){
			tnodo *nodoaeliminar=actual;
			if(anterior == NULL)
				*lista=(*lista)->sig;
			else
				anterior->sig=actual->sig;
			actual=actual->sig;
			delete(nodoaeliminar);
		} else {
			anterior=actual;
			actual=actual->sig;
		}
		eliminarOcurrenciasRecursiva(lista, actual, anterior, elemento);
	}
}

void invertirLista(tlista *lista){
	// Complejidad O(n) ya que va eliminando todos los elementos de la lista e insetandolos en otra
	tlista lAux=NULL;
	tnodo *ptrAux=*lista;
	while(ptrAux != NULL){
		insertar(&lAux, ptrAux->elemento);
		eliminarPrimero(lista);
		ptrAux=*lista;
	}
	*lista=lAux;
}

void intercambiaNodos(tlista *lista, int e1, int e2){
	/* Complejidad O(n), donde n sera la longitud que haya que recorrer de la lista hasta el
	 * elemento que mas alejado este
	 */
	tnodo *pe1=NULL, *panteriore1=NULL, *pe2=NULL, *panteriore2=NULL;
	if((! esVacia(*lista))&&(e1 != e2)){
		pe1=*lista;
		while(pe1->elemento != e1){
			panteriore1=pe1;
			pe1=pe1->sig;
		}
		pe2=*lista;
		while(pe2->elemento != e2){
			panteriore2=pe2;
			pe2=pe2->sig;
		}
		if(panteriore1 == NULL)
			*lista=pe1;
		if(panteriore2 == NULL)
			*lista=pe2;
		panteriore1->sig=pe2;
		panteriore2->sig=pe1;
		tnodo *paux=pe1->sig;
		pe1->sig=pe2->sig;
		pe2->sig=paux;
		paux=NULL;
	}
}
