´´´ABAP
  
  " EQART list box
  SELECT * FROM 'searchhelptablename' INTO TABLE @DATA(lt_eqarts).

  DATA lt_vrm_values TYPE vrm_values.

  lt_vrm_values = VALUE #( FOR ls_eqart IN lt_eqarts ( text = ls_eqart-eqart ) ).
  INSERT VALUE #( ) INTO lt_vrm_values INDEX 1.

  SORT lt_vrm_values BY text.

  CALL FUNCTION 'VRM_SET_VALUES'
    EXPORTING
      id              = 'GS_EQUIDATEN-EQART'
      values          = lt_vrm_values
    EXCEPTIONS
      id_illegal_name = 1
      OTHERS          = 2.
  IF sy-subrc <> 0.
    MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
            WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
  ENDIF.

  ´´´
