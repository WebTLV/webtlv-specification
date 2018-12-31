#
# Makefile for the WebTLV specification
#
# This makefile, written in only the finest GNU make, produces the
# specification documents and schema files that make up the WebTLV
# standard release package.
#
# Requirements:
#
#   sphinx - document processing tools
#     We use the sphinx-build frontend to produce documents.
#     Backend tooling should be provided for html and latexpdf.
#
#   asn1c - open compiler for ASN.1
#     We use the asn1c compiler for schema verification.
#
#   asn1ate - ASN.1 compiler for pyasn1
#     We use asn1ate to produce a non-normative pyasn1 mapping.
#

# Root directory for output
OUTPUT ?= output

# Various programs
ASN1C ?= asn1c
ASN1ATE ?= asn1ate
CPP ?= cpp

# Sphinx
SPHINX_BUILD   ?= sphinx-build
SPHINX_DOCS    ?= html latexpdf
SPHINX_OPTIONS ?=

# Encoding specification
ENCODING_SOURCE  := encoding
ENCODING_OUTPUT  := ${OUTPUT}/encoding
ENCODING_TARGETS := $(addprefix encoding-,${SPHINX_DOCS})

# Transport specification
TRANSPORT_SOURCE  := transport
TRANSPORT_OUTPUT  := ${OUTPUT}/transport
TRANSPORT_TARGETS := $(addprefix transport-,${SPHINX_DOCS})

# MIME information
MIME_SOURCE  := mime
MIME_OUTPUT  := ${OUTPUT}/mime
MIME_SOURCES := $(sort $(wildcard ${MIME_SOURCE}/*.xml))
MIME_COPIED  := $(addprefix ${MIME_OUTPUT}/,$(notdir ${MIME_SOURCES}))
MIME_TARGETS := ${MIME_COPIED}

# Schema files
SCHEMA_SOURCE   := schema
SCHEMA_OUTPUT   := ${OUTPUT}/schema/asn1
SCHEMA_COMBINED := ${SCHEMA_OUTPUT}/webtlv-combined.asn1
SCHEMA_CHECKED  := ${SCHEMA_OUTPUT}/webtlv-checked.asn1
SCHEMA_PYASN1   := ${OUTPUT}/schema/pyasn1/webtlv.py
SCHEMA_SOURCES  := $(sort $(wildcard ${SCHEMA_SOURCE}/*.asn1))
SCHEMA_COPIED   := $(addprefix ${SCHEMA_OUTPUT}/,$(notdir ${SCHEMA_SOURCES}))
SCHEMA_DERIVED  := ${SCHEMA_COMBINED} ${SCHEMA_CHECKED} ${SCHEMA_PYTHON}
SCHEMA_TARGETS  := ${SCHEMA_COPIED} ${SCHEMA_DERIVED}

# Default target
all: schema mime encoding transport
.PHONY: all

# Encoding specification
encoding: ${ENCODING_TARGETS} ${TRANSPORT_SPECIFICATION}
.PHONY: encoding

# Transport specification
transport: ${TRANSPORT_TARGETS} ${TRANSPORT_SPECIFICATION}
.PHONY: transport

# MIME information
mime: ${MIME_TARGETS}
.PHONY: mime

# Schema files
schema: ${SCHEMA_TARGETS}
.PHONY: schema

# Cleanup target
clean:
	rm -rf ${OUTPUT}
.PHONY: clean

# Run sphinx for encoding specification
encoding-%: phony ${SCHEMA_TARGETS} ${MIME_TARGETS}
	$(SPHINX_BUILD) -M $* "$(ENCODING_SOURCE)" "$(ENCODING_OUTPUT)" $(SPHINX_OPTIONS) $(O)

# Run sphinx for transport specification
transport-%: phony ${SCHEMA_TARGETS}
	$(SPHINX_BUILD) -M $* "$(TRANSPORT_SOURCE)" "$(TRANSPORT_OUTPUT)" $(SPHINX_OPTIONS) $(O)

# Copy mime info files to output directory
${MIME_OUTPUT}/%.xml: ${MIME_SOURCE}/%.xml
	mkdir -p $(dir $@)
	cp $< $@

# Copy schema files to output directory
${SCHEMA_OUTPUT}/%.asn1: ${SCHEMA_SOURCE}/%.asn1
	mkdir -p $(dir $@)
	cp $< $@

# Produce combined ASN.1 schema
${SCHEMA_COMBINED}: ${SCHEMA_SOURCES}
	mkdir -p $(dir $@)
	cat $? > $@

# Produce checked ASN.1 schema
${SCHEMA_CHECKED}: ${SCHEMA_SOURCES}
	mkdir -p $(dir $@)
	${ASN1C} -FE $? > $@

# Produce pyasn1 schema
${SCHEMA_PYASN1}: ${SCHEMA_CHECKED}
	mkdir -p $(dir $@)
	( ${ASN1ATE} $< | grep -v '^import ' > $@ )

# Pseudo-target for marking targets phony
phony:
.PHONY: phony

