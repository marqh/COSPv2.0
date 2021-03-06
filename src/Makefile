.SUFFIXES : .F .f .c .o .a .f90 .f95 .F90
########################################################################
# COSP Makefile
########################################################################
# Compiler and flags
# Intel (ifort)
F90      = ifort
F90FLAGS = -O3 #-fpp#-pg # -traceback -check bounds
F90_LIB  = /usr/local/ifort

# Portland Group (pgf90)
#F90      = pgf90
#F90FLAGS = -Mpreprocess -O0 -g
#F90_LIB  = /usr/local/pgf90

SRC_PATH = src/
OBJ_PATH = obj/

# Libraries
INC      = /usr/local/include
LIB      = /usr/local/lib
# Simulator directories
SIM_PATH     = simulator
RT_PATH      = $(SIM_PATH)/rttov
RS_PATH      = $(SIM_PATH)/quickbeam
CS_PATH      = $(SIM_PATH)/actsim
LLNL_PATH    = $(SIM_PATH)/llnl
ISCCP_PATH   = $(SIM_PATH)/icarus-scops-4.1-bsd
MISR_PATH    = $(SIM_PATH)/MISR_simulator
MODIS_PATH   = $(SIM_PATH)/MODIS_simulator
PARASOL_PATH = $(SIM_PATH)/parasol

# RTTOV setup
RTTOV_PATH       = /Projects/Clouds/dswales/RTTOV/rttov_11.3
RTTOV_LIB_PATH   = $(RTTOV_PATH)/lib 
RTTOV_INC_PATH   = $(RTTOV_PATH)/include 
RTTOV_MOD_PATH   = $(RTTOV_PATH)/mod
RTTOV_LIBS       = -lrttov11_mw_scatt -lrttov11_brdf_atlas -lrttov11_emis_atlas \
                   -lrttov11_other -lrttov11_parallel -lrttov11_coef_io        \
                   -lrttov11_main

OBJS =  cosp_kinds.o cosp_constants.o cosp_cloudsat_interface.o cosp_config.o  \
        prec_scops.o cosp.o cosp_stats.o quickbeam.o parasol.o                 \
        lidar_simulator.o icarus.o cosp_calipso_interface.o                    \
        cosp_isccp_interface.o cosp_misr_interface.o MISR_simulator.o          \
        cosp_modis_interface.o modis_simulator.o cosp_rttov_interfaceSTUB.o        \
        cosp_rttovSTUB.o cosp_parasol_interface.o

%.o: $(SIM_PATH)/%.F90
	@echo $(F90) $(F90FLAGS) -c $<
	$(F90) $(F90FLAGS) -c $<
	@echo "-----------------------------"

%.o: %.F90
	@echo $(F90) $(F90FLAGS) -c $<
	$(F90) $(F90FLAGS) -c $<
	@echo "-----------------------------"

# Dependencies
cosp.o                    : cosp_kinds.o cosp_modis_interface.o cosp_constants.o                   \
                            cosp_rttov_interfaceSTUB.o cosp_misr_interface.o cosp_isccp_interface.o    \
                            cosp_calipso_interface.o cosp_cloudsat_interface.o cosp_stats.o        \
			    cosp_parasol_interface.o cosp_rttovSTUB.o cosp_rttov_interfaceSTUB.o       \
                            quickbeam.o MISR_simulator.o lidar_simulator.o parasol.o icarus.o
cosp_config.o             : cosp_kinds.o
cosp_stats.o              : cosp_kinds.o cosp_config.o
cosp_calipso_interface.o  : cosp_kinds.o lidar_simulator.o
cosp_cloudsat_interface.o : cosp_kinds.o cosp_config.o quickbeam.o
cosp_isccp_interface.o    : cosp_kinds.o icarus.o
cosp_misr_interface.o     : cosp_kinds.o 
cosp_modis_interface.o    : cosp_kinds.o cosp_config.o modis_simulator.o
cosp_rttov_interfaceSTUB.o    : cosp_kinds.o cosp_config.o cosp_rttovSTUB.o
cosp_parasol_interface.o  : cosp_kinds.o 
cosp_rttovSTUB.o            : cosp_kinds.o cosp_config.o cosp_constants.o
MISR_simulator.o          : cosp_kinds.o cosp_config.o cosp_stats.o
modis_simulator.o         : cosp_kinds.o cosp_config.o cosp_stats.o
lidar_simulator.o         : cosp_kinds.o cosp_config.o cosp_stats.o
icarus.o	          : cosp_kinds.o cosp_constants.o  cosp_stats.o
parasol.o                 : cosp_kinds.o cosp_config.o cosp_constants.o

install:
	make
	mv *.mod *.o $(OBJ_PATH)
clean:	
	rm -f $(OBJ_PATH)*.mod $(OBJ_PATH)*.o fort.* *.mod *.o

icarus.o : $(ISCCP_PATH)/icarus.F90
	$(F90) $(F90FLAGS) -c $<

quickbeam.o: $(RS_PATH)/quickbeam.F90
	$(F90) $(F90FLAGS) -c $<

MISR_simulator.o : $(MISR_PATH)/MISR_simulator.F90
	$(F90) $(F90FLAGS) -c $<

modis_simulator.o : $(MODIS_PATH)/modis_simulator.F90 
	$(F90) $(F90FLAGS) -c $<

cosp_rttov_interfaceSTUB.o : simulator/cosp_rttov_interfaceSTUB.F90
	$(F90) $(F90FLAGS) -c -I $(RTTOV_INC_PATH) -I $(RTTOV_MOD_PATH) $<

cosp_rttovSTUB.o : $(RT_PATH)/cosp_rttovSTUB.F90 
	$(F90) $(F90FLAGS) -c -I $(RTTOV_INC_PATH) -I $(RTTOV_MOD_PATH) $<

lidar_simulator.o : $(CS_PATH)/lidar_simulator.F90
	$(F90) $(F90FLAGS) -c $<

parasol.o : $(PARASOL_PATH)/parasol.F90
	$(F90) $(F90FLAGS) -c $<

cosp_constants.o :  hooks/cosp_constants.F90
	$(F90) $(F90FLAGS) -c $<

cosp_kinds.o : hooks/cosp_kinds.F90
	$(F90) $(F90FLAGS) -c $<

mo_rng.o : rng/mo_rng.F90
	$(F90) $(F90FLAGS) -c $<

cosp_config.o :  cosp_config.F90
	$(F90) $(F90FLAGS) -c $<

cosp.o :  cosp.F90
	$(F90) $(F90FLAGS) -c $<

cosp_stats.o :  cosp_stats.F90
	$(F90) $(F90FLAGS) -c $<
