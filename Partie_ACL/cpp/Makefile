# Makefile pour projet Graphes - compilation multi-programmes avec tests

CXX = g++
CC = gcc
CXXFLAGS = -Wall -Wextra -std=c++17 -Iinclude -IModeles/include -IGraphes/include -IOutils/include -Ilib -I../../Partie_Algo/code/include
CFLAGS = -I../../Partie_Algo/code/include
DEBUG_FLAGS = -g -O0

SRC_DIR = src
INC_DIR = include
BIN_DIR = bin
ALGO_SRC_DIR = ../../Partie_Algo/code/src
ALGO_SOURCES = algo_dyn.c fptas.c gestion_robot.c instance.c jackson.c list.c schedule.c state.c utils.c
ALGO_OBJS = $(patsubst %.c, $(BIN_DIR)/%.o, $(ALGO_SOURCES))

# Création des dossiers
$(shell mkdir -p $(BIN_DIR))

# Liste tous les .cpp dans src/
SOURCES = $(wildcard *.cpp)
# Pour chaque source, un exécutable correspondant dans bin/
EXECUTABLES = $(patsubst %.cpp,$(BIN_DIR)/%,$(SOURCES))

# Règle spéciale pour Serveur avec les sources C de Partie_Algo
bin/Serveur: bin/Serveur.o $(ALGO_OBJS)
	$(CXX) $(CXXFLAGS) -o $@ $^

# Compilation des objets C
$(BIN_DIR)/%.o: $(ALGO_SRC_DIR)/%.c
	$(CC) $(CFLAGS) -c $< -o $@

# Compilation de Serveur.o
bin/Serveur.o: Serveur.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

# Compilation normale de tous
all: $(EXECUTABLES)

# Compilation d'un seul fichier (ex: make bin/test_AElement)
$(BIN_DIR)/%: %.cpp
	$(CXX) $(CXXFLAGS) -o $@ $<

# Debug mode (ajoute -g -O0 pour Valgrind)
debug: CXXFLAGS += $(DEBUG_FLAGS)
debug: clean all

# Exécuter un programme spécifique
run:
	@if [ -z "$(prog)" ]; then \
		echo "Utilisation : make run prog=nom_du_fichier_sans_.cpp"; \
		echo "Exemple : make run prog=TestGElement"; \
		exit 1; \
	fi
	$(MAKE) $(BIN_DIR)/$(prog)
	./$(BIN_DIR)/$(prog)

# Valgrind sur un programme spécifique (mode complet)
valgrind: debug
	@if [ -z "$(prog)" ]; then \
		echo "Utilisation : make valgrind prog=nom_du_fichier_sans_.cpp"; \
		exit 1; \
	fi
	valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./$(BIN_DIR)/$(prog)

# Valgrind lite (moins verbeux)
valgrind-lite: debug
	@if [ -z "$(prog)" ]; then \
		echo "Utilisation : make valgrind-lite prog=nom_du_fichier_sans_.cpp"; \
		exit 1; \
	fi
	valgrind --leak-check=full --show-leak-kinds=all ./$(BIN_DIR)/$(prog)

# Nettoyage
clean:
	rm -rf $(BIN_DIR)/* *.o

.PHONY: all debug run valgrind valgrind-lite clean