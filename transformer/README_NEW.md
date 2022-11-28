#Variable Declaration
MODEL=transformer
HPARAMS=transformer_base
LENGTH="max_length=5000,max_target_seq_length=256,max_input_seq_length=256"
PROBLEM=mimic_discharge_summaries
DATA_DIR=$BASE/data/t2t/data
TRAIN_DIR=$BASE/data/t2t/output
USR_DIR=$BASE/transformer
EVAL_STEPS = 50
LOCAL_EVAL_FREQUENCY = 200
TRAIN_STEPS = 4000
CONTEXT = h-gae-d-p-m-l
USR_DIR=$BASE/t2t
DATA_DIR_CONTEXT = $BASE/data/t2t/$context/data
TRAIN_DIR_CONTEXT = $BASE/data/t2t/$context/output
TRAIN_STEPS_CONTEXT = 5000
EVAL_USE_TEST_SET = True
EVAL_STEPS_EVAL = 100
SCHEDULE = evaluate
BEAM_SIZE=3
ALPHA=0.7
DBS=3
EXTRA_LEN=55
DATA_DIR_DECODER=$BASE/data/t2t/data
TRAIN_DIR_DECODER=$BASE/data/t2t/output
DECODE_FILE_0=$BASE/data/preprocessed/decoder_output.txt


# Training
t2t-trainer \
    --data_dir=$DATA_DIR \
    --problem=$PROBLEM \
    --model=$MODEL \
    --hparams_set=$HPARAMS \
    --hparams=$LENGTH \
    --output_dir=$TRAIN_DIR \
    --t2t_usr_dir=$USR_DIR \
    --eval_steps=$EVAL_STEPS \
    --local_eval_frequency=$LOCAL_EVAL_FREQUENCY \
    --train_steps=$TRAIN_STEPS \
    --warm_start_from=$TRAIN_DIR

# Training Context
t2t-trainer \
    --data_dir=$DATA_DIR_CONTEXT \
    --problem=$PROBLEM \
    --model=$MODEL \
    --hparams_set=$HPARAMS \
    --hparams=$LENGTH \
    --output_dir=$TRAIN_DIR_CONTEXT \
    --t2t_usr_dir=$USR_DIR \
    --train_steps=$TRAIN_STEPS_CONTEXT \
    --eval_steps=$EVAL_STEPS\


# Evaluate
t2t-trainer \
    --t2t_usr_dir=$USR_DIR \
    --problem=$PROBLEM \
    --model=$MODEL \
    --hparams_set=$HPARAMS \
    --data_dir=$DATA_DIR \
    --output_dir=$TRAIN_DIR \
    --eval_use_test_set=$EVAL_USE_TEST_SET \
    --eval_steps=$EVAL_STEPS_EVAL \
    --schedule=$SCHEDULE \

#Decoder
t2t-decoder \
    --t2t_usr_dir=$USR_DIR \
    --data_dir=$DATA_DIR_DECODER \
    --problem=$PROBLEM \
    --model=$MODEL \
    --hparams_set=$HPARAMS \
    --hparams=$HPARAMS_OVERRIDE \
    --output_dir=$TRAIN_DIR_DECODER \
    --decode_hparams="beam_size=$BEAM_SIZE,alpha=$ALPHA,batch_size=$DBS,extra_length=$EXTRA_LEN" \
    --decode_from_file=$DECODE_FILE \
    --decode_to_file=$TRAIN_DIR_DECODER/tgt-decoded-0.txt &
