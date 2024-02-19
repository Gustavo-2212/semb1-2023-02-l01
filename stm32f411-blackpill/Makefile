CC = arm-none-eabi-gcc
RM = rm -rf

OBJDIR = build
DEPDIR = .deps

CFLAGS   = -g -mcpu=cortex-m4 -mthumb -O0 -Wall
DEPFLAGS = -MMD -MP -MF $(DEPDIR)/$*.d

SRCS = startup.c main.c

OBJS = $(patsubst %, $(OBJDIR)/%.o, $(basename $(SRCS)))
$(shell mkdir -p $(dir $(OBJS)) > /dev/null)

DEPS = $(patsubst %, $(DEPDIR)/%.d, $(basename $(SRCS)))
$(shell mkdir -p $(dir $(DEPS)) > /dev/null)


.PHONY: all
all: $(OBJS)

$(OBJDIR)/%.o: %.c $(DEPDIR)/%.d
	$(CC) -c $(CFLAGS) $(DEPFLAGS) $< -o $@

$(DEPS):

-include $(DEPS)

.PHONY: clean
clean:
	$(RM) $(OBJDIR) $(DEPDIR)