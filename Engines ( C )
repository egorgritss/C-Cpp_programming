

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <assert.h>

#define LIST_BY_YEAR       0
#define LIST_BY_TYPE       1

#define TYPE_MAX           100
#define SETUP_MAX          100

typedef struct TEngine {
    struct TEngine *m_Next;
    struct TEngine *m_Prev;
    int m_Year;
    char m_Type[TYPE_MAX];
    int m_Setup[SETUP_MAX];
} TENGINE;

typedef struct TArchive {
    struct TArchive *m_Next;
    struct TArchive *m_Prev;
    TENGINE *m_Engines;
} TARCHIVE;

TENGINE *createEngine(const char *type,
                      int year) {
    TENGINE *res = (TENGINE *) malloc(sizeof(*res));
    res->m_Next = NULL;
    res->m_Prev = NULL;
    res->m_Year = year;
    strncpy(res->m_Type, type, sizeof(res->m_Type));
    for (int i = 0; i < SETUP_MAX; i++)
        res->m_Setup[i] = 0;
    return res;
}


/**----------------------ENGINES SORT-----------------------------*/

TARCHIVE *sortEngineByType(TARCHIVE *archive, TENGINE *engine) {                      
    TENGINE *insertPlace = archive->m_Engines;
    while (insertPlace->m_Next != NULL
           && strcmp(insertPlace->m_Next->m_Type, engine->m_Type) < 0) {
        insertPlace = insertPlace->m_Next;
    }

    if (strcmp(insertPlace->m_Type, engine->m_Type) >= 0) {
        if (insertPlace == archive->m_Engines) {
            engine->m_Next = insertPlace;
            engine->m_Prev = insertPlace->m_Prev;
            insertPlace->m_Prev = engine;
            archive->m_Engines = engine;
        } else {
            engine->m_Next = insertPlace;
            engine->m_Prev = insertPlace->m_Prev;
            insertPlace->m_Prev = engine;
            if (insertPlace->m_Prev)
                insertPlace->m_Prev->m_Next = engine;
            insertPlace = engine;
        }
    } else {
        engine->m_Next = insertPlace->m_Next;
        if (insertPlace->m_Next)
            insertPlace->m_Next->m_Prev = engine;
        engine->m_Prev = insertPlace;
        insertPlace->m_Next = engine;
    }

    return archive;
}

TARCHIVE *sortEngineByYear(TARCHIVE *archive, TENGINE *engine) {
    TENGINE *insertPlace = archive->m_Engines;
    while (insertPlace->m_Next != NULL
           && insertPlace->m_Next->m_Year < engine->m_Year) {
        insertPlace = insertPlace->m_Next;
    }

    if (insertPlace->m_Year >= engine->m_Year) {
        if (insertPlace == archive->m_Engines) {
            engine->m_Next = insertPlace;
            engine->m_Prev = insertPlace->m_Prev;
            insertPlace->m_Prev = engine;
            archive->m_Engines = engine;
        } else {
            engine->m_Next = insertPlace;
            engine->m_Prev = insertPlace->m_Prev;
            insertPlace->m_Prev = engine;
            if (insertPlace->m_Prev)
                insertPlace->m_Prev->m_Next = engine;
            insertPlace = engine;
        }
    } else {
        engine->m_Next = insertPlace->m_Next;
        if (insertPlace->m_Next)
            insertPlace->m_Next->m_Prev = engine;
        engine->m_Prev = insertPlace;
        insertPlace->m_Next = engine;

    }

    return archive;
}

/**---------------------------------------------------------------*/



/**--------------------ARCHIVE SORTING----------------------------*/

TARCHIVE *sortArchiveByYear(TARCHIVE *list, TARCHIVE *newArch) {                      /**MB OK*/
    TARCHIVE *insertPlace = list;
    while (insertPlace->m_Next != NULL
           && insertPlace->m_Next->m_Engines->m_Year < newArch->m_Engines->m_Year) {
        insertPlace = insertPlace->m_Next;
    }

    if (insertPlace->m_Engines->m_Year >= newArch->m_Engines->m_Year) {
        newArch->m_Next = insertPlace;
        newArch->m_Prev = insertPlace->m_Prev;
        insertPlace->m_Prev = newArch;
        if (insertPlace == list) {
            list = newArch;
        } else {
            insertPlace = newArch;
        }
    } else {
        newArch->m_Next = insertPlace->m_Next;
        if (insertPlace->m_Next)
            insertPlace->m_Next->m_Prev = newArch;
        insertPlace->m_Next = newArch;
        newArch->m_Prev = insertPlace;
    }


    return list;
}

TARCHIVE *sortArchiveByType(TARCHIVE *list, TARCHIVE *newArch) {
    TARCHIVE *insertPlace = list;

    while (insertPlace->m_Next != NULL
           && strcmp(insertPlace->m_Next->m_Engines->m_Type, newArch->m_Engines->m_Type) < 0) {
        insertPlace = insertPlace->m_Next;
    }

    if (strcmp(insertPlace->m_Engines->m_Type, newArch->m_Engines->m_Type) >= 0) {
        newArch->m_Next = insertPlace;
        newArch->m_Prev = insertPlace->m_Prev;
        insertPlace->m_Prev = newArch;
        if (insertPlace == list) {
            list = newArch;
        } else {
            insertPlace = newArch;
        }
    } else {
        newArch->m_Next = insertPlace->m_Next;
        if (insertPlace->m_Next)
            insertPlace->m_Next->m_Prev = newArch;
        insertPlace->m_Next = newArch;
        newArch->m_Prev = insertPlace;
    }

    return list;
}

/**---------------------------------------------------------------*/


/**------------------------ADD ARCHIVE----------------------------*/

TARCHIVE *addByYear(TARCHIVE *list, TENGINE *engine) {
    TARCHIVE *listSearch = list;
    int flag = 0;
    while (listSearch != NULL) {
        if (listSearch->m_Engines->m_Year == engine->m_Year) {
            listSearch = sortEngineByType(listSearch, engine);
            flag = 1;
            break;
        }
        listSearch = listSearch->m_Next;
    }
    if (flag == 0) {
        TARCHIVE *addEl = (TARCHIVE *) malloc(sizeof(TARCHIVE));
        addEl->m_Engines = engine;
        addEl->m_Next = NULL;
        addEl->m_Prev = NULL;
        list = sortArchiveByYear(list, addEl);
    }
    return list;
}

TARCHIVE *addByType(TARCHIVE *list, TENGINE *engine) {
    TARCHIVE *listSearch = list;
    int flag = 0;
    while (listSearch != NULL) {
        if (strcmp(listSearch->m_Engines->m_Type, engine->m_Type) == 0) {
            listSearch = sortEngineByYear(listSearch, engine);
            flag = 1;
            break;
        }
        listSearch = listSearch->m_Next;
    }
    if (flag == 0) {
        TARCHIVE *addEl = (TARCHIVE *) malloc(sizeof(TARCHIVE));
        addEl->m_Engines = engine;
        addEl->m_Next = NULL;
        addEl->m_Prev = NULL;
        list = sortArchiveByType(list, addEl);
    }
    return list;
}

/**---------------------------------------------------------------*/

TARCHIVE *AddEngine(TARCHIVE *list, int listBy, TENGINE *engine) {


    if (list == NULL) {
        TARCHIVE *addEl = (TARCHIVE *) malloc(sizeof(TARCHIVE));
        addEl->m_Engines = engine;
        addEl->m_Next = NULL;
        addEl->m_Prev =NULL;
        list = addEl;
        return list;
    }

    if (listBy == LIST_BY_YEAR) {
        list = addByYear(list, engine);
        return list;
    }

    if (listBy == LIST_BY_TYPE) {
        list = addByType(list, engine);
        return list;
    }

    return list;

}

void DelArchive(TARCHIVE *list) {
    TARCHIVE *tmpArch;
    TENGINE *tmpEng;
    while (list) {
        while (list->m_Engines) {
            tmpEng = list->m_Engines;
            list->m_Engines = list->m_Engines->m_Next;
            free(tmpEng);
        }
        tmpArch = list;
        list = list->m_Next;
        free(tmpArch);
    }


}

TARCHIVE *ReorderArchive(TARCHIVE *list, int listBy) {
    TARCHIVE *newList;
    newList = NULL;

    TARCHIVE *listPtr = list;
    TENGINE *tmp;

    int flag = 0;
    while (listPtr) {
        TENGINE *add = listPtr->m_Engines;
        while (add) {
            tmp = add;
            if (tmp->m_Next) {
                while (tmp->m_Next->m_Next) {
                    tmp = tmp->m_Next;
                }
                tmp->m_Next->m_Prev=NULL;
                newList = AddEngine(newList, listBy, tmp->m_Next);
                tmp->m_Next = NULL;
                flag++;
            } else {
                newList = AddEngine(newList, listBy, tmp);
                add = NULL;
            }


        }
        listPtr = listPtr->m_Next;
    }


    TARCHIVE *freePtr;
    while (list) {
        freePtr = list;
        list = list->m_Next;
        free(freePtr);
    }


    return newList;
}


//tests

int main(int argc,
         char *argv[]) {
    TARCHIVE *a;

    a = NULL;
    a = AddEngine(a, LIST_BY_YEAR, createEngine("TDI 1.9", 2010));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2010
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next == NULL
            && a->m_Next == NULL);
    a = AddEngine(a, LIST_BY_YEAR, createEngine("TDI 1.9", 2005));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next == NULL);
    a = AddEngine(a, LIST_BY_YEAR, createEngine("TSI 1.2", 2010));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Type, "TSI 1.2")
            && a->m_Next->m_Engines->m_Next->m_Prev == a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Next->m_Next == NULL
            && a->m_Next->m_Next == NULL);
    a = AddEngine(a, LIST_BY_YEAR, createEngine("TDI 2.0", 2005));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next->m_Year == 2005
            && !strcmp(a->m_Engines->m_Next->m_Type, "TDI 2.0")
            && a->m_Engines->m_Next->m_Prev == a->m_Engines
            && a->m_Engines->m_Next->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Type, "TSI 1.2")
            && a->m_Next->m_Engines->m_Next->m_Prev == a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Next->m_Next == NULL
            && a->m_Next->m_Next == NULL);
    a = AddEngine(a, LIST_BY_YEAR, createEngine("MPI 1.4", 2005));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "MPI 1.4")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next->m_Year == 2005
            && !strcmp(a->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Engines->m_Next->m_Prev == a->m_Engines
            && a->m_Engines->m_Next->m_Next->m_Year == 2005
            && !strcmp(a->m_Engines->m_Next->m_Next->m_Type, "TDI 2.0")
            && a->m_Engines->m_Next->m_Next->m_Prev == a->m_Engines->m_Next
            && a->m_Engines->m_Next->m_Next->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Type, "TSI 1.2")
            && a->m_Next->m_Engines->m_Next->m_Prev == a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Next->m_Next == NULL
            && a->m_Next->m_Next == NULL);
    a = ReorderArchive(a, LIST_BY_TYPE);
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "MPI 1.4")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2005
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Next->m_Prev == a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Next->m_Next == NULL
            && a->m_Next->m_Next
            && a->m_Next->m_Next->m_Prev == a->m_Next
            && a->m_Next->m_Next->m_Engines
            && a->m_Next->m_Next->m_Engines->m_Year == 2005
            && !strcmp(a->m_Next->m_Next->m_Engines->m_Type, "TDI 2.0")
            && a->m_Next->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next->m_Next
            && a->m_Next->m_Next->m_Next->m_Prev == a->m_Next->m_Next
            && a->m_Next->m_Next->m_Next->m_Engines
            && a->m_Next->m_Next->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Next->m_Next->m_Engines->m_Type, "TSI 1.2")
            && a->m_Next->m_Next->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Next->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next->m_Next->m_Next == NULL);
    DelArchive(a);

    a = NULL;
    a = AddEngine(a, LIST_BY_TYPE, createEngine("TDI 1.9", 2010));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2010
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next == NULL
            && a->m_Next == NULL);
    a = AddEngine(a, LIST_BY_TYPE, createEngine("TDI 1.9", 2005));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Engines->m_Next->m_Prev == a->m_Engines
            && a->m_Engines->m_Next->m_Next == NULL
            && a->m_Next == NULL);
    a = AddEngine(a, LIST_BY_TYPE, createEngine("TSI 1.2", 2010));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Engines->m_Next->m_Prev == a->m_Engines
            && a->m_Engines->m_Next->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Type, "TSI 1.2")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next == NULL);
    a = AddEngine(a, LIST_BY_TYPE, createEngine("TDI 2.0", 2005));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "TDI 1.9")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Engines->m_Next->m_Prev == a->m_Engines
            && a->m_Engines->m_Next->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2005
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 2.0")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next
            && a->m_Next->m_Next->m_Prev == a->m_Next
            && a->m_Next->m_Next->m_Engines
            && a->m_Next->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Next->m_Engines->m_Type, "TSI 1.2")
            && a->m_Next->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next->m_Next == NULL);
    a = AddEngine(a, LIST_BY_TYPE, createEngine("MPI 1.4", 2005));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "MPI 1.4")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2005
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Next->m_Prev == a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Next->m_Next == NULL
            && a->m_Next->m_Next
            && a->m_Next->m_Next->m_Prev == a->m_Next
            && a->m_Next->m_Next->m_Engines
            && a->m_Next->m_Next->m_Engines->m_Year == 2005
            && !strcmp(a->m_Next->m_Next->m_Engines->m_Type, "TDI 2.0")
            && a->m_Next->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next->m_Next
            && a->m_Next->m_Next->m_Next->m_Prev == a->m_Next->m_Next
            && a->m_Next->m_Next->m_Next->m_Engines
            && a->m_Next->m_Next->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Next->m_Next->m_Engines->m_Type, "TSI 1.2")
            && a->m_Next->m_Next->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Next->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next->m_Next->m_Next == NULL);
    a = AddEngine(a, LIST_BY_TYPE, createEngine("TDI 1.9", 2010));
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "MPI 1.4")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2005
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Next->m_Prev == a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Next->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Next->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Next->m_Next->m_Prev == a->m_Next->m_Engines->m_Next
            && a->m_Next->m_Engines->m_Next->m_Next->m_Next == NULL
            && a->m_Next->m_Next
            && a->m_Next->m_Next->m_Prev == a->m_Next
            && a->m_Next->m_Next->m_Engines
            && a->m_Next->m_Next->m_Engines->m_Year == 2005
            && !strcmp(a->m_Next->m_Next->m_Engines->m_Type, "TDI 2.0")
            && a->m_Next->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next->m_Next
            && a->m_Next->m_Next->m_Next->m_Prev == a->m_Next->m_Next
            && a->m_Next->m_Next->m_Next->m_Engines
            && a->m_Next->m_Next->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Next->m_Next->m_Engines->m_Type, "TSI 1.2")
            && a->m_Next->m_Next->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Next->m_Next->m_Engines->m_Next == NULL
            && a->m_Next->m_Next->m_Next->m_Next == NULL);
    a = ReorderArchive(a, LIST_BY_YEAR);
    assert (a
            && a->m_Prev == NULL
            && a->m_Engines
            && a->m_Engines->m_Year == 2005
            && !strcmp(a->m_Engines->m_Type, "MPI 1.4")
            && a->m_Engines->m_Prev == NULL
            && a->m_Engines->m_Next->m_Year == 2005
            && !strcmp(a->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Engines->m_Next->m_Prev == a->m_Engines
            && a->m_Engines->m_Next->m_Next->m_Year == 2005
            && !strcmp(a->m_Engines->m_Next->m_Next->m_Type, "TDI 2.0")
            && a->m_Engines->m_Next->m_Next->m_Prev == a->m_Engines->m_Next
            && a->m_Engines->m_Next->m_Next->m_Next == NULL
            && a->m_Next
            && a->m_Next->m_Prev == a
            && a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Prev == NULL
            && a->m_Next->m_Engines->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Type, "TDI 1.9")
            && a->m_Next->m_Engines->m_Next->m_Prev == a->m_Next->m_Engines
            && a->m_Next->m_Engines->m_Next->m_Next->m_Year == 2010
            && !strcmp(a->m_Next->m_Engines->m_Next->m_Next->m_Type, "TSI 1.2")
            && a->m_Next->m_Engines->m_Next->m_Next->m_Prev == a->m_Next->m_Engines->m_Next
            && a->m_Next->m_Engines->m_Next->m_Next->m_Next == NULL
            && a->m_Next->m_Next == NULL);
    DelArchive(a);

    return 0;
}
