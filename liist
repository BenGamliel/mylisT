
#ifndef UNTITLED6_LIST_H
#define UNTITLED6_LIST_H
#ifndef nullptr
#define nullptr 0
#endif //nullptr

#include <string>
#include <iostream>
#include <stdexcept>





class ElementNotFound : public std::runtime_error {
public:
    ElementNotFound() : std::runtime_error("Element not found") {}
};



template <typename T>
class List{
    class Node{
        T* data;
        bool tail;
        Node* next;
        Node* prev;
        /** operator == for node
         * @param node to compare with
         * @return true if data and tail are equal. otherwise - false.
         */
        bool operator==(Node& node) const{ ///check only data and tail
            return (*data == *(node.data) && tail == node.tail);
        }
        /** operator != for node
         * @param node to compare with
         * @return false if data and tail are equal. otherwise - true.
         */
        bool operator!=(Node& node) const{
            return (!(*this == node));
        }
    public:
        friend class List;
        friend class Iterator;
        /**constructor for node
         * @param data1 -value to insert
         * @param next1 -value to insert
         * @param prev1 -value to insert
         * @param tail1 -value to insert
         */
        Node(const T& data1,Node* next1, Node* prev1, bool tail1): tail(tail1){
            this->data=new T(data1);
            this->next=next1;
            this->prev=prev1;
        }
        /**constructor for dummy node
         */
        Node(){
            this->tail=true;
            this->data = nullptr;
            this->next= nullptr;
            this->prev= nullptr;
        }
        /** destructor of node - delete data (created with new)
         */
        ~Node(){
            if (!tail) {
                delete(data);
            }
        }
    };
    Node* head;
    int size;
    /**swaps 2 nodes in list
     * @param elem1 -node 1 to switch
     * @param elem2 -node 2 to switch
     */
    void swap(Node* elem1, Node* elem2){
        T* tempData=elem1->data;
        elem1->data=elem2->data;
        elem2->data=tempData;
    }
public:
    /** iterator class **/
    class Iterator{
        Node* ptr;
        List* list;
    public:
        friend class List;
        /**constructor for iterator
         * @param ptr -value to insert
         * @param list -value to insert
         */
        Iterator(Node* ptr, List* list){
            this->ptr=ptr;
            this->list=list;
        }
        /** operator++ for iterator (for it++)
         * @return - updated iterator
         */
        Iterator& operator++(){
            if(this != nullptr)
                this->ptr=this->ptr->next;
            return *this;
        }
        /** operator++ for iterator (for ++it)
         * @return - updated iterator
         */
        Iterator operator++(int){
            Iterator result=*this;
            if(this != nullptr)
                this->ptr=this->ptr->next;
            return result;
        }
        /** operator-- for iterator (for it--)
         * @return - updated iterator
         */
        Iterator& operator--(){
            if(this != nullptr)
                this->ptr=this->ptr->prev;
            return *this;
        }
        /** operator-- for iterator (for --it)
         * @return - updated iterator
         */
        Iterator operator--(int){
            Iterator result=*this;
            --*this;
            return result;
        }
        /** dereference for iterator- returns the data of the pointed node
         * throws mtm::ListExceptions::ElementNotFound- if iterator points
         * to illegal place in list
         */
        T* operator*() const{
            if(this->ptr == nullptr)
                throw ElementNotFound();
            if(this->ptr->tail)
                throw ElementNotFound();
            T* dataPtr=this->ptr->data;
            return dataPtr;
        }
        /** operator== for iterator- returns true if both belongs to same
         * list, and points to the same data (or both dummy)
         * @param iterator to compare with
         * @return true if equal. otherwise-false
         */
        bool operator==(Iterator iterator) const{
            if(this->ptr == nullptr || iterator.ptr == nullptr)
                return false;
            if(this->list != iterator.list)
                return false;
            if(this->ptr->tail && iterator.ptr->tail)
                return true;
            return (this->ptr->data == iterator.ptr->data);
        }
        bool operator!=(Iterator iterator) const{
            return (!(*this == (iterator)));
        }
    }; /****/
    /** constructor for list without parameters
     */
    List(){
        head=new Node();
        size=0;
    }
    /** copy constructor for list- initialize values with values of param list
     * @param list1
     */
    List(const List& list1){
        head=new Node;
        size=0;
        for(Node* cur_node=list1.head; !(cur_node->tail);
            cur_node=cur_node->next){
            this->insert(*(cur_node->data));
        }
    }
    /** get the begin of the list
     * @return - iterator pointing to the begin of the list
     */
    Iterator begin(){
        Iterator it(this->head,this);
        return it;
    }
    /** get the end of the list
     * @return - iterator pointing to the end of the list (not to any real
     * object of list)
     */
    Iterator end(){
        Iterator it=begin();
        while(!it.ptr->tail)
            it.operator++();
        return it;
    }
    /*** check if given list is equal to "this" list
     * @param list - the list to compare to
     * @return - true if the lists are equal. otherwise - false.
     */
    bool operator==(const List& list) const{
        Node* ptr1=head;
        Node* ptr2=list.head;
        while(!ptr1->tail){
            if(ptr2->tail)
                return false;
            if(*ptr1 != *ptr2)
                return false;
            ptr1=ptr1->next;
            ptr2=ptr2->next;
        }
        return ptr2->tail;
    }
    /** check if given list is not equal to "this" list
     * @param list -the list to copmare to "this"
     * @return - true if not equal. otherwise - false.
     */
    bool operator!=(const List& list) const{
        return !(*this == list);
    }
    /** assignment operator for list
     * @param list - the list to copy to "this"
     * @return - a copy list of given list. if the given list equals to
     * "this", return "this"
     */
    List& operator=(const List& list){
        if (*this==list)
            return *this;
        bool flag=false;
        Iterator it=this->end();
        if(!this->head->tail){
            flag=true;
            it--;
        }
        Iterator begin=this->begin();
        while(it != begin) {
            this->remove(it);
            it=this->end();
            it--;
        }
        if(flag)
            this->remove(begin);
        this->head->tail=true;
        this->head->next= nullptr;
        this->head->prev= nullptr;
        (*this).size=0;
        for(Node* cur_node=list.head; !(cur_node->tail);
            cur_node=cur_node->next){
            this->insert(*(cur_node->data));
        }
        return *this;
    }
    /** insert element to the given place in list
     * @param data - the  data to insert
     * @param iterator - points to the wanted place to insert to (inserts data
     * before this place)

    void insert(const T& data, Iterator iterator){
        if(iterator.list != this)
            throw ElementNotFound();
        if(iterator.ptr ==NULL)
            insert(data);
        else if(iterator.ptr->tail)
            insert(data);
        else {
            T data1=data;
            Node* newNode= new Node(data1,iterator.ptr,
                                    iterator.ptr->prev,false);
            if(iterator.ptr->prev == nullptr)
                this->head=newNode;
            else
                iterator.ptr->prev->next=newNode;
            iterator.ptr->prev=newNode;
            this->size = this->size + 1;
        }
    }*/
    /**insert element to the start of the list
     * @param data - the data to insert
     */
    void insert(const T& data){
        Iterator it=this->begin();
        Node* newNode=new Node(data,it.ptr,nullptr,false);
        head->prev=newNode;

        this->head=newNode;
        this->size=this->size+1;
    }
    /**remove function for list
     * @param iterator - to the node to be deleted
     */
    void remove(Iterator iterator){
        if(iterator.list != this)
            throw ElementNotFound();
        if(iterator.ptr->tail)
            throw ElementNotFound();
        if(iterator.ptr->prev == nullptr){
            iterator.ptr->next->prev=nullptr;
            this->head=iterator.ptr->next;
        }
        else {
            iterator.ptr->prev->next = iterator.ptr->next;
            iterator.ptr->next->prev=iterator.ptr->prev;
        }
        delete (iterator.ptr);
        this->size=this->size-1;
    }
    /**find function for list - finds specific node according to given
     * predicate function object
     * @tparam Predicate - function object to find node
     * @return - iterator to wanted object
     */
    template <class Predicate>
    Iterator find(const Predicate& predicate){
        Node* ptr=this->head;
        while (!ptr->tail) {
            if (predicate(*(ptr->data))){
                Iterator it(ptr,this);
                return it;
            }
            ptr=ptr->next;
        }
        Iterator it=this->end();
        return it;
        //changed to fit the find function of a dynamic Hash list
    }
    /** sort function for list
     * @param compare - function object, used to sort the list
     */
    template <class Compare>
    void sort(const Compare& compare){
        Node* elem1Node=this->head;
        if(elem1Node->tail)
            return;
        Node* elem2Node;
        while(!elem1Node->tail) {
            elem2Node=elem1Node->next;
            while (!elem2Node->tail) {
                if (!(compare(*(elem1Node->data), *(elem2Node->data))))
                    swap(elem1Node, elem2Node);
                elem2Node=elem2Node->next;
            }
            elem1Node=elem1Node->next;
        }
    }
    /** get size of nodes in list
     * @return - size of list (without dummy node)
     */
    int getSize() const{
        return this->size;
    }
    /**destructor of list- delete all relevant fields, include data and nodes
     */
    ~List(){
        bool flag=false;
        Iterator it=this->end();
        if(!this->head->tail){
            flag=true;
            it--;
        }
        Iterator begin=this->begin();
        while(it != begin) {
            this->remove(it);
            it=this->end();
            it--;
        }
        if(flag)
            this->remove(begin);
        it=this->end();
        delete(it.ptr);
    }
};
#endif //UNTITLED6_LIST_H
