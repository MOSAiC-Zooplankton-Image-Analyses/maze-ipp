# MAZE Image Processing Pipeline (MAZE-IPP)

## 0. Prerequisites

```sh
# Install pyecotaxa (development branch)
pip install git+https://github.com/moi90/pyecotaxa.git@transfer

# Acquire EcoTaxa login token
# (This stores the token in the current directory)
# TODO: Local/User flag
pyecotaxa login

# Get a full copy of EcoTaxa taxonomy tree
# (The tree is stored in <taxonomy-fn>)
pyecotaxa pull-taxonomy --format=<raw|yaml|...> <taxonomy-fn>
```

## 1. Pull EcoTaxa data

```sh
# Pull one (or multiple) projects
# (This stores the exported archives in the data/ directory.)
# Arguments:
#	-d: Working directory
#	--with-images: Include images in the export.
#	<project_id*>: A space-delimited list of project IDs
pyecotaxa pull -d data --with-images <project_id*>
```

## 2. Process data

### 2.1. Configure pipeline

Create `task.yaml` according to the following template and fill in the missing information.

```yaml
...
```

### 2.2. Run pipeline

```sh
# (This writes the resulting files to ...)
pipeline data/task.yaml
```

## 3. Annotate using MorphoCluster

### 3.1. Import

### 3.2. Annotation

### 3.3. Export

```sh
flask export ...
```

## 4. Map MorphoCluster labels to EcoTaxa categories

```sh
pyecotaxa map-categories --taxonomy <taxonomy-fn>
```

## 4a. Merge annotations back into project data

(This step might be necessary if multiple EcoTaxa projects were annotated jointly in MorphoCluster or if previous annotations should not be overwritten.)

```sh
# (This creates <output-fn> with )
# Arguments:
#	--annotation-only: Only `object_annotation_*` columns are contained in the output.
# 	--predicted-only: Only objects with annotation status "predicted" are contained in the output.
#		(This avoids overwriting already validated annotations.)
pyecotaxa merge --annotation-only --predicted-only <export-archive-fn> annotations.tsv <output-fn>
```

## 5. Push annotated data back to EcoTaxa

```sh
pyecotaxa push --project-id <project_id> --update-annotations <data-fn>
```

