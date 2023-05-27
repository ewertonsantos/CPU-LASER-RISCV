# CPU-LASER-RISCV
#LOAD WORD
lw rd, imm(rs1)
lw t1, 0(s1) #lw register_target, immediate(register_source1)
# register_source = memory_address
# =========================LW MACHINE CODE==========================
#   imm 11:0        rs1 19:15   funct3 14:12    rd 11:7     op 6:0   #
 xxxx xxxx xxxx |     xxxxx   |     010      |   xxxxx   | 000 0011
#    12 bits         5 bits        3 bits       5 bits      7 bits


#STORE WORD
sw rs2, imm(rs1)
sw  t1, 0(s1) #lw register_source2, immediate(register_source1) # register_source = memory_address
# =============================SW MACHINE CODE==============================
#   imm 11:5     rs2 24:20     rs1 19:15   funct3 14:12    imm 4:0     op 6:0  #
    xxxx xxx  |   xxx xx    |   xxx xx   |      010     |   xxx xx  | 010 0011
#    7 bits       5 bits        5 bits        3 bits        5 bits     7 bits


#R-TYPES
    add rd, rs1, rs2
    add t0, t1, t2;  # t0(register_target) = t1(register_source) + t2(register_source)
    # ================================ ADD MACHINE CODE ===================================
    #   funct7 31:25      rs2 24:20       rs1 19:15     funct3 14:12     rd 11:7     op 6:0  #
           0000 000    |    xx xxx    |     xx xxx    |      000      |   xxx xx   | 0110 011 
    #      7 bits          5 bits           5 bits        3 bits         5 bits      7 bits

    sub rd, rs1, rs2
    sub t0, t1, t2;  # t0(register_target) = t1(register_source) - t2(register_source)
    # ================================ SUB MACHINE CODE ===================================
    #   funct7 31:25      rs2 24:20       rs1 19:15     funct3 14:12     rd 11:7     op 6:0  #
           0100 000    |    xx xxx    |     xx xxx    |      000      |   xxx xx   | 0110 011 
    #      7 bits          5 bits           5 bits        3 bits         5 bits      7 bits

    and rd, rs1, rs2
    and t0, t1, t2;  # t0(register_target) = t1(register_source) && t2(register_source)
    # ================================ AND MACHINE CODE ===================================
    #   funct7 31:25      rs2 24:20       rs1 19:15     funct3 14:12     rd 11:7     op 6:0   #
           0000 000    |    xx xxx    |     xx xxx    |      111      |   xxx xx   | 0110 011 
    #      7 bits          5 bits           5 bits         3 bits        5 bits      7 bits

    or rd, rs1, rs2
    or t0, t1, t2;  # t0(register_target) = t1(register_source) || t2(register_source)
    # ================================ OR  MACHINE CODE ===================================
    #   funct7 31:25      rs2 24:20       rs1 19:15     funct3 14:12     rd 11:7     op 6:0  #
           0000 000    |    xx xxx    |     xx xxx    |      110      |   xxx xx   | 0110 011 
    #      7 bits          5 bits           5 bits        3 bits         5 bits      7 bits

#I-TYPE
    addi rd, rs1, imm
    addi t0, t1, imm;  # t0(register_target) = t1(register_source) + imm(immediate constant)
    # ================================ ADDi  MACHINE CODE ===================================
    #          imm 11:0           rs1 19:15     funct3 14:12     rd 11:7     op 6:0  #
           xxxx xxxx xxxx    |     xx xxx    |      000      |   xxx xx   | 0010 011 
    #           12 bits            5 bits          3 bits        5 bits      7 bits

    andi rd, rs1, imm
    andi t0, t1, imm;  # t0(register_target) = t1(register_source) && imm(immediate constant)
    # ================================ ANDi  MACHINE CODE ===================================
    #          imm 11:0           rs1 19:15     funct3 14:12     rd 11:7     op 6:0  #
           xxxx xxxx xxxx    |     xx xxx    |      111      |   xxx xx   | 0010 011 
    #           12 bits            5 bits          3 bits        5 bits      7 bits

    ori rd, rs1, imm
    ori t0, t1, imm;  # t0(register_target) = t1(register_source) || imm(immediate constant)
    # ================================  ORi  MACHINE CODE ===================================
    #          imm 11:0           rs1 19:15     funct3 14:12     rd 11:7     op 6:0  #
           xxxx xxxx xxxx    |     xx xxx    |      110      |   xxx xx   | 0010 011 
    #           12 bits            5 bits          3 bits        5 bits      7 bits

    slti rd, rs1, imm
    slti t0, t1, imm; # if t1 < imm then t0 = 1 else t0 = 0
    # ================================  slti  MACHINE CODE ===================================
    #          imm 11:0           rs1 19:15     funct3 14:12     rd 11:7     op 6:0  #
           xxxx xxxx xxxx    |     xx xxx    |      010      |   xxx xx   | 0010 011 
    #           12 bits            5 bits          3 bits        5 bits      7 bits


#CONDITIONAL JUMP
beq rs1, rs2, label # label: text indicating instruction address
beq t0, t1, label; # if t0 == t1 then jump immediate's times (immediate's times forward or backward)
# ================================  BEQ  MACHINE CODE ===================================
    #        imm 12,10:5       rs1 19:15     funct3 14:12     imm 4:1,11    op 6:0  #
               xxxx xxx    |     xx xxx    |      000      |     xxx xx   | 1100 011 
    #           7 bits          5 bits          3 bits          5 bits      7 bits



#INCONDITIONAL JUMP
jal target  # jump to target(memory address, typicaly a label) and save position to ra (return register) register
jal rd, imm  # jump immediate's times (immediate's times forward or backward) (memory address, typicaly a label) and save position to ra (return register) register
# ================================  JAL  MACHINE CODE ===================================
    #                     imm 20,10:1,11,19:12                 rd 11:7     op 6:0  #
                           xxxx xxxx xxxx  xxx          |       xxx xx  |  1100 011 
    #                            20 bits                        5 bits      7 bits




